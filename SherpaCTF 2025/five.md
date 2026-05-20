# Five - Log Analysis

<div align="center"><img src="https://github.com/user-attachments/assets/b1b4d1bc-4fa2-4075-979c-90fc3c2d0d90" alt="image" width="563"></div>

We are give a folder with a few resources, including logs

<div align="center"><img src="https://github.com/user-attachments/assets/20e9ba45-ae11-48ee-8dc8-075ff285cedc" alt="image" width="563"></div>

Opening defacement.html gives us this:

<div align="center"><img src="https://github.com/user-attachments/assets/a056bfbb-26b8-40ea-94ed-da6956c086ca" alt="image" width="563"></div>

Let's take a look at the log files. I decided to start with the vpn\_logs first

<div align="center"><img src="https://github.com/user-attachments/assets/adf111c9-15aa-4132-ab88-0fc5f6b9b58f" alt="image" height="100" width="578"></div>

<div align="center"><img src="https://github.com/user-attachments/assets/f79e6936-f292-4960-836e-fbb87462d313" alt="image"></div>

We can see that there was a login from "anon123" at 2024-11-12, so maybe we can start by looking in 2024 logs.

<div align="center"><img src="https://github.com/user-attachments/assets/dfc21ead-a292-45b0-af93-f7e9919d5c7b" alt="image"></div>

This file `nusa_shell.php` is probably the malicious file. The flag is SHERPACTF25{nusa\_shell.php}
