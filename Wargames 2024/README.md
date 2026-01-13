# Wargames 2024 Writeup - DaemonHunter


## Introduction

Username: DaemonHunter

Team: Penem

Division: Student

Hi! This my writeup on my solves for Wargames 2024. I'm still quite new to CTF, so I did not get many solves. I still had fun participating and I can't wait to do the same next year!




## Forensic


### I Cant Manipulate People
This one is fairly simple, it's one pcap file. 

After inscpecting the pcap in Wireshark, when following the ICMP traffic, you can see that one character of the flag is located in ech packet.

Knowing this, we can write a tshark command to extract the data from the end of each ICMP packet

![image](https://github.com/user-attachments/assets/ae7c9faf-fab2-4be6-b3ad-da13631e5137)

Flag found!



### Unwanted Meow
We are presented with a SHREDDED file. 

View in HxD hex editor

![image](https://github.com/user-attachments/assets/d5888bfc-62c1-4695-a267-0050b42eb0bf)

According the maigc number, this is a jpg file

The hex is riddled with "meow"

We can do a simple search and replace for the word meow and replace it with nothing

NOTE: HxD didnt managed to get all the "meow", so I had to open the hex in VSCode and manually find some meows and remove them

![flag1](https://github.com/user-attachments/assets/6f21b78e-00f9-4442-867b-3fc16fbc1439)




## Misc


### Christmas GIFt
We are give a huge (and long) GIF file

Using stegsolve's frame browser, I just browsed the frames until I found the one with the flag

![image](https://github.com/user-attachments/assets/a3bad3a0-eea8-4dc6-9e7f-53f50b39aadc)



### Invisible Ink
We are given a GIF that countsdown to a flag reveal, but we can see it

Using stegsolve's frame browser, I just browsed the frames until I found the one two weird frames

![image](https://github.com/user-attachments/assets/45690809-475e-43b0-a314-5c7cc66aad3c)
![image](https://github.com/user-attachments/assets/17aea0d4-b5a5-4cd0-b4da-45221268bdea)

After playing with the color maps in stegsolve, I got this out of the two frames

![image](https://github.com/user-attachments/assets/55243fbc-a970-4094-b73b-6bb28cb73da6)
![image](https://github.com/user-attachments/assets/c991c55d-f412-4cb6-9c69-1680630cc66f)

I layered the images in GIMP and played with the opacity, finally getting this:

![image](https://github.com/user-attachments/assets/d1b9da11-b207-4916-8805-aa76c743ab0d)




## Crypto


### Credentials
Question: We found a leak of a blackmarket website's login credentials. Can you find the password of the user osman and successfully decrypt it?

We are given 2 .txt files, passwd and user

All we have to do is find "osman" in user.txt and then find the password of the same line in passwd.txt

![image](https://github.com/user-attachments/assets/62a0cda8-84b3-4bfc-8b9d-4282ae605b11)

![image](https://github.com/user-attachments/assets/8e5f5a5e-e30e-492f-8f50-95b6516dc937)

Its encrypted with some sort of ROT cipher. After playing around with the key value in CyberChef, we get the flag

![image](https://github.com/user-attachments/assets/81ce5fce-c084-4358-9851-aaafe5536cf7)




## Reverse


### Stones
Question: When Thanos snapped his fingers, half of the flag was blipped. We need the Avengers to retrieve the other half. There's no flag in the movie, but there is a slash flag on the server

We are given a weird file, stones.whatdis

After running the "file" command, we find that it is an EXE

![image](https://github.com/user-attachments/assets/7f5c134b-a3b0-4ebd-adbd-e38b706f09e6)

After changing the extension to .exe, I tried running it in my Windows VM.

The file fails to run, but not before opening the cmd and flashing some sort of Python error (It was really quick so I couldn't capture it)

So, I decided to unpack the .exe file back into python files so I can take a look at the code

I followed this guide: https://www.youtube.com/watch?v=jmC-FKNRdvk

![image](https://github.com/user-attachments/assets/ac9dd31e-5058-4b20-b708-189320399ecc)

Instead of changing the magic number of the .pyc file, I used an online converter https://pylingual.io/ to convert it into readble code

![image](https://github.com/user-attachments/assets/d8881c79-b72f-411a-a9ec-a5fa5fe29b55)

This code sends a carfted request to a server containing a part of the flag and a date 

There's a few things here:
- A part of the flag
- A server IP

Navigating to the IP with a "/flag" at the end returns this

![image](https://github.com/user-attachments/assets/3d94e734-a02e-4eb2-ae9f-0ed3caabf3f8)

This youtube link is for an Avengers Endgame clip, with the key "Upload Date"

![image](https://github.com/user-attachments/assets/4417ab65-1ec3-47f0-969b-ddae2706ccd0)

With all this information, I made some changes to the code

![image](https://github.com/user-attachments/assets/5f07697d-61f9-41b4-a178-9c0030670a55)

I changed the date the code was using to send the request to the upload date of the youtube video

![image](https://github.com/user-attachments/assets/575cec93-83b1-4249-8266-a446faf6b282)

Got the flag!




## Conclusion
Well, that's all the flags I got this time. It's not much but it was fun. I'm looking forward to try again next year. Thanks for reading!






