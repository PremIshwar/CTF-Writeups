# Twelve - View Basket

<p align="center" width="100%">
<img width="528" height="108" alt="image" src="https://github.com/user-attachments/assets/e5bdd76a-1449-4b51-b3ea-488ac615a45e" />
<img width="392" height="154" alt="image" src="https://github.com/user-attachments/assets/c5e1b6d6-3003-4e34-a8ea-44a760910e25" />
</p>

We need to access the basket of another user. I logged in as the user I created earlier and captured the ```/basket``` request with BurpSuite.

<p align="center" width="100%">
<img width="784" height="572" alt="image" src="https://github.com/user-attachments/assets/b67b898d-b0b9-4738-b461-5874063119ad" />
</p>

At the GET request, we can see that its requesting for ```/rest/basket/6``` , so maybe this is the user's id. I tried to change the 6 to a 5 and forwarded the request.

<p align="center" width="100%">
<img width="1139" height="468" alt="image" src="https://github.com/user-attachments/assets/e61c5a16-66d1-4c71-ae01-cbe5840399ff" />
</p>

Doing so gives us the flag: e6982b34b6734ceadd28e5019b251f929a80b815
