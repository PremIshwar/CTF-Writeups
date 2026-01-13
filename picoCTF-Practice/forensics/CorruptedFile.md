# Corrupted File (Easy)

_This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?_
\
We are give a file with no extenstion or metadata.
\
\
<img width="493" height="73" alt="image" src="https://github.com/user-attachments/assets/aac8dd0b-8fb6-44f1-82ae-7b975040b451" />
\
\
After dumping the hex, the first few bytes tells us that it might be a JPEG file. Maybe its magic number is messed up?
\
\
<img width="715" height="144" alt="image" src="https://github.com/user-attachments/assets/efd6b060-490b-4d51-a5b5-7db428dd1af2" />
\
\
I opened the file in Bless and put in the correct magic number
\
\
<img width="313" height="86" alt="image" src="https://github.com/user-attachments/assets/e19204cf-be93-46a9-96a3-a19bd3175fdb" />
\
\
Great! Now the file is can be opened as a JPEG and we got the flag!
\
\
<img width="586" height="226" alt="image" src="https://github.com/user-attachments/assets/7098f25e-42da-48e8-9355-79262bf57490" />

