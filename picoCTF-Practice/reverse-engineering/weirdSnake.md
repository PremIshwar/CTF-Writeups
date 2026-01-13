# weirdSnake
_I have a friend that enjoys coding and he hasn't stopped talking about a snake recently He left this file on my computer and dares me to uncover a secret phrase from it. Can you assist?_
\
\
We're given a file called _snake_ and when reading it's contents we see that it's actually Assembly.
\
\
<img width="563" height="485" alt="image" src="https://github.com/user-attachments/assets/e0cbe97a-b6e6-48cc-a869-35ee99252e1e" />
\
\
Upon further inspection, it actually looks like disassembled python code.
\
\
<img width="1042" height="487" alt="image" src="https://github.com/user-attachments/assets/e487c07a-b3c5-4f41-ac75-dcbc430067c7" />
\
\
I found a tool online that could convert Assembly into Python code (https://codingfleet.com/code-converter/assembly/python/) After some cleaning up and adding a print, here's the code I got:
```python
input_list = [4, 54, 41, 0, 112, 32, 25, 49, 33, 3, 0, 0, 57, 32, 108, 23, 48, 4, 9, 70, 7, 110, 36, 8, 108, 7, 49, 10, 4, 86, 43, 108, 122, 14, 2, 71, 62, 115, 88, 78]

key_str = "tJ_o3"

key_list = [ord(char) for char in key_str]

while len(key_list) < len(input_list):
    key_list.extend(key_list)
   
result = [a ^ b for a, b in zip(input_list, key_list)]

result_text = ''.join(map(chr, result))

print(result_text)
```
\
<img width="451" height="71" alt="image" src="https://github.com/user-attachments/assets/ac9dfe16-ecd5-4253-8a6e-5932d212090b" />
\
\
Hmmm, we got something that looks close to a flag, but not quite. Let's take a closer look at how the code works.
```python
input_list = [4, 54, 41, 0, 112, 32, 25, 49, 33, 3, 0, 0, 57, 32, 108, 23, 48, 4, 9, 70, 7, 110, 36, 8, 108, 7, 49, 10, 4, 86, 43, 108, 122, 14, 2, 71, 62, 115, 88, 78]

key_str = "tJ_o3"

key_list = [ord(char) for char in key_str]
```
We have an input list of 40 decimal numbers and a key string. The _key_list_ is just the ASCII value of each character in the string.
```python
while len(key_list) < len(input_list):
    key_list.extend(key_list)
```
This while loop adds the values of _key_list_ to itself until it length is the same as the input list. This means the _key_list_ is extended 10 times.
```python
result = [a ^ b for a, b in zip(input_list, key_list)]
```
This line has a bit going on. First the _input_list_ values and _key_list_ values are zipped. This means a list of tuples are created, where [(input_list[0], key_list[0]), (input_list[1], key_list[1]),.......] for all values in the lists. Then, each tuple is XORed and the resulting values are stored in _result_.  
```python
result_text = ''.join(map(chr, result))
```
This just combines the values into and string and stores it in _result_text_.
\
\
**p|voCTSnN0tJfO_cz[fus${g_s{Uke_&%a13t,7}**
\
\
Looking at the flag we got, we can see that some characters like p, C, T are correct. Let's look at the first 5 characters (the length of the key). Since we know the flag starts with "picoC" and we get "p|voC", the 2nd and 3rd chars are wrong. So, what if we change the 2nd and 3rd chars of the key text until we get "picoC"?
\
\
I wrote this tiny script that goes through all ASCII values and prints the characters that result in 'i' and 'c' after being XORed.
```python
for x in range(127):
    
    i = 54 ^ x
    c = 41 ^ x

    if i == 105:
        print("This gives 'i': ", chr(x))

    if c == 99:
        print("This gives 'c': ", chr(x))
```
\
<img width="452" height="108" alt="image" src="https://github.com/user-attachments/assets/8e88a3ce-659b-4947-9b73-5b187b720e34" />
\
\
Wait... Do I just have to switch them around? After switching the key from "tJ_o3" to "t_Jo3" I got the flag
\
\
<img width="445" height="79" alt="image" src="https://github.com/user-attachments/assets/a9e62113-0b73-41b0-84e5-970a89656c16" />
\
\
At this point, I looked back at the Assembly file to see if I overlooked something and sure enough, I find this:
\
\
<img width="437" height="190" alt="image" src="https://github.com/user-attachments/assets/0250bf03-12e5-4a06-aa51-c560209ca3ab" />
\
\
'_' is added infront on 'J'. So the resulting string would have been "t_Jo3" to begin with. I think I flipped the chars when I was cleaning up the code 🤦‍♂️. I guess I didn't have to do this whole side quest to get the flag after all. Oh well, lesson learnt.

