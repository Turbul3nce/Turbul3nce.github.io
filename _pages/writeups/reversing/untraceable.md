
---
layout: ctf-layout
title: Untraceable
permalink: /writeups/reversing/untraceable
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/97ajfg.jpg" title="ThreeKeys" width="90%" />
</p>

##### Difficulty: <b>Easy</b> 

##### Description:
You need access to the Halliday Journals to gather vital info to crack the next clue. However, security has been tightened, and only VIPs with a password may enter. Can you crack the new security system?

##### Challenge: 
The program seems to ask the user for a password, checks if it's correct, and based on that input, either prints the flag or a message indicating tampering. The binary also includes a check to detect tracing or debugging attempts using ptrace(). However, the core challenge here is static analysis—no need for runtime debugging tricks.

##### Solution

##### Analyzing The Binary:
The main logic resides in the main() function. First, let's look at how the password is stored and checked:

```C
local_58[0x20] = 'S';
local_58[0x21] = 'u';
local_58[0x22] = 'p';
// continues to fill out "SuperSecretPassword-DoNotRead!"
```

The password is hardcoded into memory starting at local_58[0x20] and is as follows:

```
SuperSecretPassword-DoNotRead!
```

This is immediately revealing and indicates that the correct password is stored in the binary itself.

##### Extracting the Flag

Assuming the correct password is provided (SuperSecretPassword-DoNotRead!), the program enters a loop that XORs the password with a key and prints the result:

```
for (local_c = 0; local_c < 0x1d; local_c = local_c + 1) {
    putchar((int)local_58[(long)(int)local_c + 0x20] ^ (uint)(byte)(&DAT_00104070)[(int)local_c]);
}
```

This loop iterates 29 times, XORing each character of the password with values from &DAT_00104070. To solve this statically, we can reverse this XOR operation in Ghidra or simply run the binary with the correct password to see the output.
<br>

After passing the password to the binary, we get the flag:

```
┌──(kali㉿kali)-[~/Desktop/rev_untraceable]
└─$ ./untraceable 
What is the password to the archive? SuperSecretPassword-DoNotRead!
HTB{0ld3st_tr1ck_1n_th3_b00k}
```
