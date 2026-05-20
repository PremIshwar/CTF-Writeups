# Library

This was the second Boot2Root challenge in this CTF, and it was slightly more challenging to exploit. Similiar to Freshman, we are give an OVA file to set up and attack

<div align="center"><img src="https://github.com/user-attachments/assets/9b808cfb-2207-4eee-a1f3-b8d9e077b241" alt="image" width="563"></div>

***

## Enumeration

Our Nmap scan showed similar results to Freshman, but this time SSH was also open (however, it did require credentials). The test on MySQL also return similar results as Freshman.

<div align="center"><img src="https://github.com/user-attachments/assets/37dac67f-39bd-43f6-83a8-d25d5be0e526" alt="image"></div>

### HTTP (Port 8080)

<div align="center"><img src="https://github.com/user-attachments/assets/2de5123e-e2d2-425d-b657-765a24e64138" alt="image" width="563"></div>

A simple website, and fuzzing the directories did not produce anything worthwhile.

### FTP (Port 21)

<div align="center"><img src="https://github.com/user-attachments/assets/fbe93722-d8d8-418d-be4b-5409f940ae14" alt="image" width="375"></div>

This time FTP allows for anonymous login. So, I tried logging in as anonymous and a blank password.

<div align="center"><img src="https://github.com/user-attachments/assets/6a75df16-2d30-4382-b3f2-b88f664ea627" alt="image" width="375"></div>

Nice.

***

## Exploitation

I looked around the filesystem and downloaded a file from `/public`.

<div align="center"><img src="https://github.com/user-attachments/assets/0833dd67-773d-40ca-8161-9ef2f0a6df29" alt="image" width="563"></div>

<img src="https://github.com/user-attachments/assets/3cda2599-d500-4325-b4ad-5a98f243d29e" alt="image" width="563">

We a password, now all we need is a username. Before doing anything, I decided to connect to SSH using the username "librarian".

<div align="center"><img src="https://github.com/user-attachments/assets/fb8dffd6-adac-414b-a043-031f759d08a9" alt="image" width="563"></div>

Oh... I did not expect that to work. Now we can read the user flag.

<div align="center"><img src="https://github.com/user-attachments/assets/165f72a6-baf8-4a07-a6f3-49f5b24e98fd" alt="image" width="563"></div>

***

## Privilege Escalation

This time, the privelege escalation was quite tricky. There was no sudo, and anythoer PE check I tried were not fruitful

<div align="center"><img src="https://github.com/user-attachments/assets/79acb9f4-e1f9-4225-beeb-176818b4d23c" alt="image" height="88" width="500"></div>

There was folder in the home directory called `/books`. It contained two files that seemed to be empty.

<div align="center"><img src="https://github.com/user-attachments/assets/e3e47e8f-5eae-4bef-a0c2-4ca7ec2da312" alt="image" height="180" width="500"></div>

At this point, I exhausted most of my options. I decided to try to run LinPEAS. I hosted the file on my machine using a python HTTP server and downloaded it on the victim's machine. After running the script, there weren't any 95% PE vectors. After scrolling through the output, I found something that stuck out.

<div align="center"><img src="https://github.com/user-attachments/assets/45bcc7a8-c5ed-4da1-aab1-738f98bad750" alt="image" height="127" width="600"></div>

Hmm... it was already more than 5 minutes since the VM has been on and there weren't any cron jobs in `/etc/crontab`. I decided to check out the directory.

<div align="center"><img src="https://github.com/user-attachments/assets/46c1010d-163f-4017-8ff0-d853dfe2d8a3" alt="image" height="63" width="600"></div>

Ok, it seems to be a .tar file with backups of the two files earlier. And its owned by root. There must be some sort of backup script running, however my earlier `ps aux` didn't catch anything. As a test, I created a test file in `~/books` to see if the backup file changes. After a minute or so, there it was.

<div align="center"><img src="https://github.com/user-attachments/assets/d4d96bcf-17f0-4a2d-a415-77d1eefb2f33" alt="image" height="113" width="500"> <img src="https://github.com/user-attachments/assets/b4942a7e-49e4-4703-8d10-02fbafdae28a" alt="image" height="142" width="500"></div>

I did some looking around and some GPT-ing about this, to check if it is a viable PE vector. Turns out, its called a cron wildcard PE. After some back and forth with ChatGPT, I finally got an exploit that worked:

```bash
echo 'cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash' > shell.sh
chmod +x shell.sh
touch -- '--checkpoint=1'
touch -- '--checkpoint-action=exec=sh shell.sh'
touch test.txt
```

When tar is executed on a directory containing files that begin with --, those filenames can be interpreted as command-line options, not just files. The exploit abuses the --checkpoint and --checkpoint-action options in GNU tar:

* `--checkpoint=1` → triggers an action after processing 1 file
* `--checkpoint-action=exec=<command>` → executes a command

When the TAR script runs as root, `shell.sh` is executed and does the following:

* Copies `/bin/bash` to `/tmp/rootbash`
* Sets the SUID bit → allows execution as root

Then, we can spawn a root shell with `/tmp/rootbash -p`

<div align="center"><img src="https://github.com/user-attachments/assets/553e76d4-f7fb-432a-a241-4e124ed92cdf" alt="image" height="205" width="600"></div>

Great! We can now read the flag file.

<div align="center"><img src="https://github.com/user-attachments/assets/203cf478-176b-4cbc-a1e3-7bd0da15dbb2" alt="image" height="278" width="600"></div>
