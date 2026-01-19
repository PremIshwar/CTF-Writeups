# Five - Log Analysis

<p align="center" width="100%">
<img width="633" height="245" alt="image" src="https://github.com/user-attachments/assets/b1b4d1bc-4fa2-4075-979c-90fc3c2d0d90" />
</p>

We are give a folder with a few resources, including logs

<p align="center" width="100%">
<img width="426" height="68" alt="image" src="https://github.com/user-attachments/assets/20e9ba45-ae11-48ee-8dc8-075ff285cedc" />
</p>


Opening defacement.html gives us this:

<p align="center" width="100%">
<img width="1119" height="723" alt="image" src="https://github.com/user-attachments/assets/a056bfbb-26b8-40ea-94ed-da6956c086ca" />
</p>


Let's take a look at the log files. I decided to start with the vpn_logs first

<p align="center" width="100%">
<img width="578" height="100" alt="image" src="https://github.com/user-attachments/assets/adf111c9-15aa-4132-ab88-0fc5f6b9b58f" />
</p>

<p align="center" width="100%">
<img width="983" height="471" alt="image" src="https://github.com/user-attachments/assets/f79e6936-f292-4960-836e-fbb87462d313" />
</p>


We can see that there was a login from "anon123" at 2024-11-12, so maybe we can start by looking in 2024 logs.

<p align="center" width="100%">
<img width="1460" height="587" alt="image" src="https://github.com/user-attachments/assets/dfc21ead-a292-45b0-af93-f7e9919d5c7b" />
</p>

This file ```nusa_shell.php``` is probably the malicious file. The flag is SHERPACTF25{nusa_shell.php}
