# Eavesdrop (Medium)
_Download this packet capture and find the flag._
\
\
We are give a PCAP file to analyze. After, opening in Wireshark, I followed a TCP stream and found this :
\
\
<img width="775" height="306" alt="image" src="https://github.com/user-attachments/assets/8a8f8deb-8e56-43a5-811d-214d9fffda18" />
\
\
Switching to a different TCP stream, stream 2, we can see that it's using port 9002 (as mentioned in the message above). The stream contents appear to be some encoded message.
\
\
<img width="1026" height="242" alt="image" src="https://github.com/user-attachments/assets/a8ebabcb-f2c6-4a0a-a607-20d55e9377b8" />
\
<img width="470" height="82" alt="image" src="https://github.com/user-attachments/assets/d5d553a6-0d27-484c-9bdf-fd75a0c0a2c2" />
\
\
I saved this into file.des3 and tried to decode with the given command, but there was an error:
\
\
<img width="782" height="137" alt="image" src="https://github.com/user-attachments/assets/e0a4d8b8-5283-4f16-8564-d23db54f2602" />
\
\
After some browsing online, I realised that I was actually saving the ASCII text into file.des3 and maybe openssl couldn't read that. I tried to switch to "Show data as RAW" in Wireshark before saving the file and tried again.
\
\
<img width="728" height="172" alt="image" src="https://github.com/user-attachments/assets/4776ae05-1817-4377-9e13-e0aab4f34045" />
\
\
Flag found!
