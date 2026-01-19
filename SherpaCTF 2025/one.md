# One - Reverse Cipher

Once again, we find the challenge on the Deskop.

<p align="center" width="100%">
<img width="948" height="582" alt="image" src="https://github.com/user-attachments/assets/deba59a6-f632-4295-a8b6-c5afb4856305" />
</p>

Now, since we don't have access to the Internet, we can't simply use any online tool to reverse this. So I wrote a simple python script that would reverse the string.

```python
cipher = '''
<CIPHER TEXT>
'''

print(cipher[::-1])
```
Note: With the luxury of the Internet, I found out that this could have been solve with a simple ```cat one.txt | rev```.

Running the script gives us the following text:

```text
Whoever finds this story next will have to think backwards before they can claim the flag.
She closed the terminal, left the file as a quiet challenge on the desktop, and locked the back door behind her.
Smiling, she whispered, 'Sometimes you do not need new tools, you just need to read things in reverse.'
but as a single continuous secret}Terz{}.
Only then did she realize that all four parts were meant to be read not as separate words,
Minutes before dawn, the terminal output froze and printed a final short note: [FLAG PART 4: _it_44}].
She suspected there might even be a final checksum or enclosure waiting somewhere.
Three fragments now, still missing something to feel complete in her mind.
It was buried in a corrupted payload header: [FLAG PART 3: {I_Kn3w].
Just when she considered giving up, a third anomaly flashed by in the capture.
Her coffee had turned cold, but the hunt for patterns pekt her wide awake.
Outside the data center, rain tapped the windows in a slow, patient rhythm.
She pinned both fragments to her virtual board, still unsure what puzzle she was solving.
'PACTF25?' she frowned. 'That is not even a word, but it feels like it wants to be combined.'
This one was clearer, almost intentional: [FLAG PART 2: PACTF25].
Hours later, a second message drifted through the flood of meaningless characters.
The night deepened, and the air conditioner hummed like a distant server farm.
Shrugging, she saved the message into a file and kept watching the incoming packets.
It did not look like a full code, more like the opening fragment of a larger key.
Hidden among the noise, she noticed something odd: [FLAG PART 1: SHER].
In that moment the console flickered, and a strange message appeared on the screen.
She muttered, 'If only I could read what the attackers read, maybe it would make sense.'
Everyone else had gone home, but the network still whispered secrets in plain text.
On the outskirts of the terminal, a lone analyst stared at the scrolling logs.
```

From which we can extract the flag and the password for user "two": SHERPACTF25{I_Kn3w_it_44}
