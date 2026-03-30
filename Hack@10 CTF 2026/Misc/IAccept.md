# I Accept
_The legal team just pushed a 50-clause update to the BlackVault User Agreement. They claim it's ironclad and that absolutely nobody actually reads these documents anyway. Prove them wrong._
\
\
We are given a link to a corporate website with a lengthy Terms and Conditions. There are a few things that stick out, but the page is sooo longgg, its really hard to see it clearly
\
\
<img width="975" height="563" alt="image" src="https://github.com/user-attachments/assets/1c7f9d4a-0c34-44bd-b597-fe0147c1d5d5" />
\
\
I copied the page source code and put it into ChatGPT to weeb out the flag fragments and these are the ones I got. 
\
\
<img width="613" height="122" alt="image" src="https://github.com/user-attachments/assets/3853194b-aff5-4c7d-82ff-e7429fcb89e2" />
\
\
<img width="603" height="159" alt="image" src="https://github.com/user-attachments/assets/146b50bf-ab9b-43eb-bec1-f36a4d3c35a8" />
\
\
Hmm... but it looks like we are still missing a part of the flag. I decided to check out other source files and found this in style.css
\
\
<img width="375" height="156" alt="image" src="https://github.com/user-attachments/assets/ff3e4dbc-a533-4341-947a-6d9ecff5e372" />
\
\
Great! We can now piece together the flag: ```hack10{f1n3_pr1nt_n3v3rr_l13ss}```
