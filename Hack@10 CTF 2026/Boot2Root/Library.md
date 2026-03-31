# Library

This was the second Boot2Root challenge in this CTF, and it was slightly more challenging to exploit. Similiar to Freshman, we are give an OVA file to set up and attack

<p align="center">
<img width="500" height="434" alt="image" src="https://github.com/user-attachments/assets/9b808cfb-2207-4eee-a1f3-b8d9e077b241" />
</p>

---

## Enumeration

Our Nmap scan showed similar results to Freshman, but this time SSH was also open (however, it did require credentials). The test on MySQL also return similar results as Freshman.

<p align="center">
<img width="800" height="366" alt="image" src="https://github.com/user-attachments/assets/37dac67f-39bd-43f6-83a8-d25d5be0e526" />
</p>

### HTTP (Port 8080)

<p align="center">
<img width="975" height="270" alt="image" src="https://github.com/user-attachments/assets/2de5123e-e2d2-425d-b657-765a24e64138" />
</p>

A simple website, and fuzzing the directories did not produce anything worthwhile.

### FTP (Port 21)

<p align="center">
<img width="500" height="234" alt="image" src="https://github.com/user-attachments/assets/fbe93722-d8d8-418d-be4b-5409f940ae14" />
</p>

This time FTP allows for anonymous login. So, I tried logging in as anonymous and a blank password.

<p align="center">
<img width="500" height="406" alt="image" src="https://github.com/user-attachments/assets/6a75df16-2d30-4382-b3f2-b88f664ea627" />
</p>

Nice.

---

## Exploitation

I looked around the filesystem and downloaded a file from ```/public```.

<p align="center">
<img width="600" height="648" alt="image" src="https://github.com/user-attachments/assets/0833dd67-773d-40ca-8161-9ef2f0a6df29" />
<img width="600" height="176" alt="image" src="https://github.com/user-attachments/assets/3cda2599-d500-4325-b4ad-5a98f243d29e" />
</p>

We a password, now all we need is a username. Before doing anything, I decided to connect to SSH using the username "librarian".

<p align="center">
<img width="600" height="604" alt="image" src="https://github.com/user-attachments/assets/fb8dffd6-adac-414b-a043-031f759d08a9" />
</p>

Oh... I did not expect that to work. Now we can read the user flag.

<p align="center">
<img width="500" height="266" alt="image" src="https://github.com/user-attachments/assets/165f72a6-baf8-4a07-a6f3-49f5b24e98fd" />
</p>

---

## Privilege Escalation

This time, the privelege escalation was quite tricky. There was no sudo, and anythoer PE check I tried were not fruitful

<p align="center">
<img width="500" height="88" alt="image" src="https://github.com/user-attachments/assets/79acb9f4-e1f9-4225-beeb-176818b4d23c" />
</p>

There was folder in the home directory called ```/books```. It contained two files that seemed to be empty.

<p align="center">
<img width="500" height="180" alt="image" src="https://github.com/user-attachments/assets/e3e47e8f-5eae-4bef-a0c2-4ca7ec2da312" />
</p>

At this point, I exhausted most of my options. I decided to try to run LinPEAS. I hosted the file on my machine using a python HTTP server and downloaded it on the victim's machine. After running the script, there weren't any 95% PE vectors. After scrolling through the output, I found something that stuck out.

<p align="center">
<img width="600" height="127" alt="image" src="https://github.com/user-attachments/assets/45bcc7a8-c5ed-4da1-aab1-738f98bad750" />
</p>

Hmm... it was already more than 5 minutes since the VM has been on and there weren't any cron jobs in ```/etc/crontab```. I decided to check out the directory.

<p align="center">
<img width="600" height="63" alt="image" src="https://github.com/user-attachments/assets/46c1010d-163f-4017-8ff0-d853dfe2d8a3" />
</p>

Ok, it seems to be a .tar file with backups of the two files earlier. And its owned by root. There must be some sort of backup script running, however my earlier ```ps aux``` didn't catch anything. As a test, I created a test file in ```~/books``` to see if the backup file changes. After a minute or so, there it was.

<p align="center">
<img width="500" height="113" alt="image" src="https://github.com/user-attachments/assets/d4d96bcf-17f0-4a2d-a415-77d1eefb2f33" />
<img width="500" height="142" alt="image" src="https://github.com/user-attachments/assets/b4942a7e-49e4-4703-8d10-02fbafdae28a" />
</p>

I did some looking around and some GPT-ing about this, to check if it is a viable PE vector. Turns out, its called a cron wildcard PE. After some back and forth with ChatGPT, I finally got an exploit that worked:


```bash
echo 'cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash' > shell.sh
chmod +x shell.sh
touch -- '--checkpoint=1'
touch -- '--checkpoint-action=exec=sh shell.sh'
touch test.txt
```

When tar is executed on a directory containing files that begin with --, those filenames can be interpreted as command-line options, not just files.
The exploit abuses the --checkpoint and --checkpoint-action options in GNU tar:

* ```--checkpoint=1``` → triggers an action after processing 1 file
* ```--checkpoint-action=exec=<command> ```→ executes a command

When the TAR script runs as root, ```shell.sh``` is executed and does the following:

* Copies ```/bin/bash``` to ```/tmp/rootbash```
* Sets the SUID bit → allows execution as root

Then, we can spawn a root shell with ```/tmp/rootbash -p```

<p align="center">
<img width="600" height="205" alt="image" src="https://github.com/user-attachments/assets/553e76d4-f7fb-432a-a241-4e124ed92cdf" />
</p>

Great! We can now read the flag file.

<p align="center">
<img width="600" height="278" alt="image" src="https://github.com/user-attachments/assets/203cf478-176b-4cbc-a1e3-7bd0da15dbb2" />
</p>
