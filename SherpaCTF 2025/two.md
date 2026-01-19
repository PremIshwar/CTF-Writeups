# Two - First Web Exploitation

<p align="center" width="100%">
<img width="531" height="153" alt="image" src="https://github.com/user-attachments/assets/5f19cc14-026e-4ab6-9363-84d2321a1c4e" />
</p>

<br>

Opening the link brings us to this website:

<p align="center" width="100%">
<img width="1000" height="734" alt="image" src="https://github.com/user-attachments/assets/7ac9f212-c2b0-4a93-92d9-05f6f70e313a" />
</p>

<br>

It was at this moment when I regretted not practicing my Web Exploitation on OWASP web applications 😞.

I tried navigating to ```/scoreboard``` but it just redirected me to the homepage. I decided to look around the user's files and that's when I found something:

<p align="center" width="100%">
<img width="638" height="364" alt="image" src="https://github.com/user-attachments/assets/963d324b-14f7-4248-a1b5-3fc73dc58172" />
<img width="250" height="826" alt="image" src="https://github.com/user-attachments/assets/28324797-f6fe-449a-af4c-01f0cc5161e7" />
</p>

<br>

It was a PDF talking about how to solve the challenges in the Juice Shop. However some stuff seemed to be redacted. After looking around, I found this page talking aboutthe challenge:

<p align="center" width="100%">
<img width="567" height="428" alt="image" src="https://github.com/user-attachments/assets/57a8c23b-82dd-4e0e-902a-fdb58b1b6078" />
</p>

<br>

So, it is guess work 🤷‍♂️. After noticing the scoreboard was actually spelt Score Board (with a space) I tried ```/score-board``` and that worked!

<p align="center" width="100%">
<img width="1397" height="781" alt="image" src="https://github.com/user-attachments/assets/c0bcec13-f955-4851-900f-fe146f96bc90" />
</p>

<br>

Clicking on the flag icon in the Score Board challenge card gives us the flag: 2614339936e8282e2f820f023d4d998a1f95e02a
