# Riddle Registry (Easy)

_Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered. Find the PDF file here Hidden Confidential Document and uncover the flag within the metadata._

This question gives us a PDF file with what looks like some redacted text:
\
\
<img width="837" height="729" alt="image" src="https://github.com/user-attachments/assets/ba6be155-810b-4e9a-a1d7-11d4eb4205b0" />
\
\
A snippet of text at the end of the file: _If you're still reading this, I’ll tell you a secret: the answer might not be here after all..._

So, most probably we don't have to do anything to the text at all. I tried looking of the flag in the file's strings and metadata instead. 
\
\
<img width="824" height="414" alt="image" src="https://github.com/user-attachments/assets/8683020d-beb5-437d-b776-dc6568ab6800" />
\
\
I found a base64 text in the metadata under the author section.
\
\
<img width="755" height="86" alt="image" src="https://github.com/user-attachments/assets/15f247a9-75bc-4ddf-841a-4dafc082d6ce" />
\
\
We found the flag! 
