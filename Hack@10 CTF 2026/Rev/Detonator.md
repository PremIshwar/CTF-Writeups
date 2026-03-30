# Detonator
_In malware analysis, you can either statically analyze the assembly codes directly, or you can create a snapshot of your sandbox and detonate it inside._
\
\
<img width="793" height="82" alt="image" src="https://github.com/user-attachments/assets/fcada87c-69ae-4f80-b8ce-5475f89c31d3" />
\
\
We are given a .exe file. I tried to run it in AnyRun, but I couldn't get any good results (not to mention to long wait time). I decided to disassemble the .exe in Ghidra and take a look at the code. The main function calls the check_flag() function:
<br><br>
```C

/* check_flag() */

void check_flag(void)

{
  int iVar1;
  undefined8 uVar2;
  basic_ostream *pbVar3;
  undefined local_c8 [48];
  basic_string<> local_98 [32];
  undefined8 local_78 [5];
  undefined local_4a;
  undefined local_49;
  basic_string<> local_48 [32];
  undefined *local_28;
  undefined *local_20;
  
  local_20 = &local_4a;
  std::__cxx11::basic_string<>::basic_string<>
            ((basic_string<> *)local_78,
             "C:\\Users\\HACK10{f4k3_fl4g_bu7_y0u_4r3_in_7h3_righ7_7r4ck}\\Desktop\\local.txt");
  std::__new_allocator<char>::~__new_allocator();
  local_28 = &local_49;
  std::__cxx11::basic_string<>::basic_string<>
            (local_98,"HACK10{f4k3_fl4g_bu7_y0u_4r3_in_7h3_righ7_7r4ck}");
  std::__new_allocator<char>::~__new_allocator();
  uVar2 = std::__cxx11::basic_string<>::c_str(local_78);
  iVar1 = stat64i32(uVar2,local_c8);
  if (iVar1 == 0) {
    pbVar3 = std::operator<<((basic_ostream *)&_ZSt4cout,"Here is the flag: HACK10{");
    md5(local_48,local_78);
    pbVar3 = std::operator<<(pbVar3,(basic_string *)local_48);
    std::operator<<(pbVar3,"}\n");
    std::__cxx11::basic_string<>::~basic_string(local_48);
  }
  else {
    std::operator<<((basic_ostream *)&_ZSt4cout,"File not found. Keep looking...\n");
  }
  std::__cxx11::basic_string<>::~basic_string(local_98);
  std::__cxx11::basic_string<>::~basic_string((basic_string<> *)local_78);
  return;
}
```
It seems like the flag is just the MD5 hash of the following string: ```C:\Users\HACK10{f4k3_fl4g_bu7_y0u_4r3_in_7h3_righ7_7r4ck}\Desktop\local.txt```
\
\
I used this python script to calculate the hash and print the flag.
<br><br>
```python
import hashlib
path_string = r"C:\Users\HACK10{f4k3_fl4g_bu7_y0u_4r3_in_7h3_righ7_7r4ck}\Desktop\local.txt"
md5_hash = hashlib.md5(path_string.encode()).hexdigest()
flag = f"HACK10{{{md5_hash}}}"
print(flag)
```
Flag: ```HACK10{be029cf0e9f2eaa5f80489343630befb}```
