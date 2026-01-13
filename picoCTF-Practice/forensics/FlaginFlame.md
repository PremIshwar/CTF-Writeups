# Flag in Flame (Easy)

_The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log. Download the encoded data here: Logs Data. Be prepared—the file is large, and examining it thoroughly is crucial ._

Here we are give a large text file which contains, as mentioned, encoded text
\
\
<img width="808" height="229" alt="image" src="https://github.com/user-attachments/assets/2d5d3a11-df59-4924-88cf-df910d38d206" />
\
<img width="325" height="78" alt="image" src="https://github.com/user-attachments/assets/ced5f024-a961-416c-bbb7-40296bb3eae2" />
\
\
The end of the file ends with an "==", which to me looks like the whole thing is probably decoded in base64. So, I decided to decode it and see what I get.
\
\
<img width="450" height="65" alt="image" src="https://github.com/user-attachments/assets/29b5ce12-e2ef-482e-a4c0-756f29e512d1" />
\
\
<img width="1100" height="223" alt="image" src="https://github.com/user-attachments/assets/a54364d1-7072-41ce-b660-159623021a07" />
\
\
Looks like gibberish, but familiar gibberish. The last time I saw something like this, I tried to cat an image file.
\
\
<img width="592" height="175" alt="image" src="https://github.com/user-attachments/assets/e9bd6bb1-bffa-4d0d-b372-8ff5fbd65095" />
\
\
Aha! So it is a PNG image! Let's try to change the file extension and open the file.
\
\
<img width="715" height="925" alt="image" src="https://github.com/user-attachments/assets/209697e4-dd9c-4eb1-8073-1ce8b9f06588" />
\
\
At the bottom of the file: 7069636F4354467B666F72656E736963735F616E616C797369735F69735F616D617A696E675F62653836303237397D  
Which is hexadecimal encoded. After putting it through CyberChef, we get the flag!
\
\
<img width="514" height="105" alt="image" src="https://github.com/user-attachments/assets/729929bb-568b-4fc4-a50e-5af5fd98e0fd" />



