# Hidden In Plainsight (Easy)

_You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag._
\
\
<img width="640" height="640" alt="image" src="https://github.com/user-attachments/assets/d90bcd42-1a06-4e7c-ac67-e4e2d3b1679f" />
\
\
Looking at the metadat, there is a comment in base64:
\
\
<img width="607" height="166" alt="image" src="https://github.com/user-attachments/assets/222f4c63-3c6d-41d7-89dc-ba1c107d428b" />
\
\
After decoding, it tells to use steghide and also gives a base64 password:
\
\
<img width="520" height="142" alt="image" src="https://github.com/user-attachments/assets/60584bb7-e6db-4e5e-a55b-9de6ad81959d" />
\
\
Using steghide to extract the flag:
\
\
<img width="511" height="82" alt="image" src="https://github.com/user-attachments/assets/bc289c1d-1c43-4315-9bb6-857fb8505390" />
\
\
<img width="509" height="76" alt="image" src="https://github.com/user-attachments/assets/ae22ca9a-ff71-4e62-bb6c-36960fab9946" />
