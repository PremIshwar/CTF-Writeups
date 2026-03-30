# Meowww
_Our Incident Response team discovered a suspicious cute image lurking within the system, hinting at a hidden payload and potential attacker activity—it's up to you to analyze the evidence and uncover the truth._
\
\
<img width="881" height="913" alt="image" src="https://github.com/user-attachments/assets/3be42732-f565-4149-a3e8-658682831261" />
\
\
We are given a JPG picture of a cat. I did my usual routine when it comes to image forensics. Exiftool, strings, Stegsolve, but nothing stuck out. I decided to try stegseek, and that's when I found something. 
\
\
<img width="975" height="272" alt="image" src="https://github.com/user-attachments/assets/c6333878-e3c3-4b9c-82d5-b0dc5e9d19fa" />
\
\
There was a hidden payload (looks like Powershell) 
<br><br>
```
(nEW-objECt  SYstem.iO.COMPreSsIon.deFlaTEStREAm( [IO.mEmORYstreAM][coNVERt]::FROMBAse64sTRING( 'UzF19/UJV7BVUMpITM42NKguMCg3LopPMU42SDGuVQIA') ,[io.COmPREssioN.coMpreSSioNmODE]::DeCoMpReSS)| %{ nEW-objECt  sYStEm.Io.StREAMrEADeR($_,[TeXT.encodiNG]::AsCii)} |%{ $_.READTOENd()})| & ( $eNV:cOmSPEc[4,15,25]-JOin'')
```
\
\
This string must be the payload we are looking for, which seems to be in base64:
\
\
```UzF19/UJV7BVUMpITM42NKguMCg3LopPMU42SDGuVQIA```
\
\
I was not familiar with this, so I asked ChatGPT (I don't have Claude set up yet) on what I should do next. Apparently this is a base 64 encoded and compressed payload. After decoding it, I used the python script that I got from GPT to decompress it and we get the flag!
\
\
<img width="975" height="272" alt="image" src="https://github.com/user-attachments/assets/6e5ca320-f163-4b02-85cf-841856b6bf48" />


