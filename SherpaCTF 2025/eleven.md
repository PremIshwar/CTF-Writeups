# Eleven - Reversing the Rubik's Cube

This challenge was a bit different, there was no instructions like the previous ones. There was only one file, Rubiks.html

<div align="center"><img src="https://github.com/user-attachments/assets/497b6829-c22b-4fe5-8d28-600dea229999" alt="image" width="563"></div>

Looks like we have to solve the Rubik's cube to get the flag. Now, I have never solved a Rubik;s cube in my life, so this was out of the question. Although, it was fascinating to see other participants get the flag by actually solving the cube. I didn't even attempt that. Since this was just a hosted file, we can make it do whatever we want. So, I decided to inspect the source code. I opened the file in Mousepad and search for "flag". Unfortunately, the flag was not stored in plaintext 😞

After some analysing, I found that this function was called when the "Check Solution" button was pressed:

<div align="center"><img src="https://github.com/user-attachments/assets/133bf6c0-408a-4c42-945e-a5f6555b903d" alt="image"></div>

This function actually calls the needed functions to display the decoded flag, the `if (_0x4cbd36)` statement just needs to return True. So I made this small change to the code so that even if `isSolved` is False, the flag is still dispalyed.

<div align="center"><img src="https://github.com/user-attachments/assets/e3d5da06-9d8c-4a54-a37d-885dc8571534" alt="image"></div>

<div align="center"><img src="https://github.com/user-attachments/assets/06be3d69-8b4d-42b9-8209-a95a1870676b" alt="image" width="563"></div>

After clicking the "Check Solution" button, we get the flag: RUBIKSCUBEMASTA
