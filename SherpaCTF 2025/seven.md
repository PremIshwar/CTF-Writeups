# Seven - More Logs

<p align="center" width="100%">
<img width="520" height="155" alt="image" src="https://github.com/user-attachments/assets/cf345cc5-b81c-498d-b06e-602b468cfbdd" />
</p>

We have to find the origin IP address of the attacker, let's look back in the 2024 server logs


<p align="center" width="100%">
<img width="668" height="67" alt="image" src="https://github.com/user-attachments/assets/a2b1a42e-dcee-4617-be33-4d73b01caf9e" />
</p>


This must be the a proxy IP the attacker used. Let's see if we can trace it back. Looking at the VPN logs, we can see that only one other user accessed the system on the same date, "nusa_guest". 

<p align="center" width="100%">
<img width="804" height="74" alt="image" src="https://github.com/user-attachments/assets/1d6964d4-b5d8-4d4e-a1b0-7a43981ed458" />
</p>

<p align="center" width="100%">
<img width="1497" height="644" alt="image" src="https://github.com/user-attachments/assets/ea794233-3dfa-451d-9deb-c1f2f94fab25" />
</p>

Looking back at the 2024 logs, the timestamps of suspicious activity actually align with the start and end time of "nusa_guest" in the VPN logs. So, this must mean that "nusa_guest" was the account the attacker used to upload the shell. The flag is the remote IP of "nusa_guest": SHERPACTF2{103.17.24.77}



