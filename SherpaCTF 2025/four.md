# Four - Confidential Document

<div align="center"><img src="https://github.com/user-attachments/assets/cbfebab2-95eb-439c-96fb-7da96d4473ed" alt="image" height="101" width="602"></div>

I had no idea where to start, so I decided to look up the challenge in the score-board to see if there were any clues

<div align="center"><img src="https://github.com/user-attachments/assets/f0f8f859-67ee-4f82-9cc2-12ae3e798694" alt="image" height="151" width="440"></div>

Hmm...sensitive data exposure. I also decided to look in the PDF found in "two", which I have to switch users to access

<div align="center"><img src="https://github.com/user-attachments/assets/be270e70-890a-478e-b0d8-f2729eac4c34" alt="image" height="152" width="614"></div>

I decided to start by opening an image in a new tab and tamper with the link:

`http://localhost:42000/assets/public/images/products/apple_juice.jpg` -> `http://localhost:42000/robots.txt`

<div align="center"><img src="https://github.com/user-attachments/assets/1082545b-9654-4a46-8fec-7c79f909e68d" alt="image" height="120" width="561"></div>

Seeing this, I tried to access `/ftp` instead.

<div align="center"><img src="https://github.com/user-attachments/assets/bca36658-3157-4ed0-8bff-874fb8d42dc2" alt="image" width="563"></div>

Great! Looks like we can access some files here and this seems to be the confidential document.

<div align="center"><img src="https://github.com/user-attachments/assets/e436c16a-81e9-4475-8224-9c2f82b39e99" alt="image" width="563"></div>

<div align="center"><img src="https://github.com/user-attachments/assets/e3d41930-f226-4bca-8256-7b032f374efc" alt="image" width="563"></div>

Returning to the score-board, we can claim our flag: 8d2072c6b0a455608ca1a293dc0c9579883fc6a5
