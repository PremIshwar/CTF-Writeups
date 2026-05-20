# Seven - More Logs

<div align="center"><img src="https://github.com/user-attachments/assets/cf345cc5-b81c-498d-b06e-602b468cfbdd" alt="image" height="155" width="520"></div>

We have to find the origin IP address of the attacker, let's look back in the 2024 server logs

<div align="center"><img src="https://github.com/user-attachments/assets/a2b1a42e-dcee-4617-be33-4d73b01caf9e" alt="image" height="67" width="668"></div>

This must be the a proxy IP the attacker used. Let's see if we can trace it back. Looking at the VPN logs, we can see that only one other user accessed the system on the same date, "nusa\_guest".

<div align="center"><img src="https://github.com/user-attachments/assets/1d6964d4-b5d8-4d4e-a1b0-7a43981ed458" alt="image"></div>

<div align="center"><img src="https://github.com/user-attachments/assets/ea794233-3dfa-451d-9deb-c1f2f94fab25" alt="image"></div>

Looking back at the 2024 logs, the timestamps of suspicious activity actually align with the start and end time of "nusa\_guest" in the VPN logs. So, this must mean that "nusa\_guest" was the account the attacker used to upload the shell. The flag is the remote IP of "nusa\_guest": SHERPACTF2{103.17.24.77}
