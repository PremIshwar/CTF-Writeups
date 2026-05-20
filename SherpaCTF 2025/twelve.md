# Twelve - View Basket

<div align="center"><img src="https://github.com/user-attachments/assets/e5bdd76a-1449-4b51-b3ea-488ac615a45e" alt="image" width="563"></div>

<img src="https://github.com/user-attachments/assets/c5e1b6d6-3003-4e34-a8ea-44a760910e25" alt="image" height="154" width="392">

We need to access the basket of another user. I logged in as the user I created earlier and captured the `/basket` request with BurpSuite.

<div align="center"><img src="https://github.com/user-attachments/assets/b67b898d-b0b9-4738-b461-5874063119ad" alt="image" width="563"></div>

At the GET request, we can see that its requesting for `/rest/basket/6` , so maybe this is the user's id. I tried to change the 6 to a 5 and forwarded the request.

<div align="center"><img src="https://github.com/user-attachments/assets/e61c5a16-66d1-4c71-ae01-cbe5840399ff" alt="image" width="563"></div>

Doing so gives us the flag: e6982b34b6734ceadd28e5019b251f929a80b815
