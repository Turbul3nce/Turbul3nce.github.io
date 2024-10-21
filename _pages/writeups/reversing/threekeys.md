---
layout: ctf-layout
title: ThreeKeys
permalink: /writeups/reversing/threekeys
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/97ajfg.jpg" title="ThreeKeys" width="90%" />
</p>

##### Difficulty: <b>Medium</b> 

##### Description:
You've gathered the three secret keys and you're ready to claim your prize. But in your rush, the keys have got mixed up - which one goes where?

##### Challenge: 
This is probably the first actual reversing challenge that I have attempted/solved during a CTF. So, it was a pretty fun learning experience. I have messed around with Ghidra in the past, but this was my first time using gdb, and becoming familiar with the debugger's syntax (ALOT of ChatGPT and Google). The objective was pretty clear: find the correct order of the keys and decrypt the hidden flag.

##### Analyzing The Binary:
The first step was to load the binary into Ghidra, a powerful reverse engineering tool, and inspect the main() function. The relevant parts of the decompiled main() function were as follows:
```C
pAVar3 = the_third_key();
decrypt(ctx, (uchar *)out, (size_t *)0x2, (uchar *)pAVar3, in_R8);
memcpy(ctx, out, 0x20);

pAVar3 = the_second_key();
decrypt(ctx, (uchar *)out, (size_t *)0x2, (uchar *)pAVar3, in_R8);
memcpy(ctx, out, 0x20);

pAVar3 = the_first_key();
decrypt(ctx, (uchar *)out, (size_t *)0x2, (uchar *)pAVar3, in_R8);
memcpy(ctx, out, 0x20);

iVar2 = memcmp(out, &DAT_00102071, 3);
if (iVar2 != 0) {
    out = "...something's wrong";
}
printf("[*] Your prize is: %s\n", out);
```
###### Key Takeaways from the Decompiled Code:
* The program sequentially calls the_third_key(), the_second_key(), and the_first_key() to obtain keys and decrypt the flag.
* After each decryption step, the decrypted result is copied into a ctx buffer using memcpy.
* A memcmp() call compares the decrypted output with a reference data section (DAT_00102071), but only checks the first 3 bytes.
It became clear that the order of keys was important, and that the program was only validating the first 3 bytes of the decrypted result, which likely represented the start of the flag (e.g., "HTB").


##### Solution 
I needed to swap the calls to these functions in the correct order: the_first_key() should use Key 1, the_second_key() should use Key 2, and the_third_key() should use Key 3.

###### Reversing the Key Order
Upon closer inspection of the the_third_key(), the_second_key(), and the_first_key() functions, it became apparent that the keys were mixed up in the main() function:
* the_third_key() actually contained what seemed to be Key 1.
* the_second_key() contained Key 2.
* the_first_key() contained Key 3.
<br>

##### Bypassing the Error Check:
But, even after swapping the keys, there was still a catch: the program only compared the first 3 bytes of the decrypted output using this line:
```C
iVar2 = memcmp(out, &DAT_00102071, 3);
```
The comparison was limited to 3 bytes, so if the flag contained more than just "HTB", it wouldn't display it.

Patch Step: Disabling the memcmp() Check
To bypass the restriction and ensure the entire buffer was printed, I needed to patch the binary. This was done in Ghidra by replacing the following assembly:
```assembly
001013ce  75 06    ; JNZ LAB_001013d6
```
with the NOP intructions:
```assembly
001013ce  48 90    ; NOP
```
This effectively disabled the error check, allowing the program to continue and print the entire decrypted buffer without checking if the first 3 bytes matched.

#### The Flag: 
After patching the binary, I exported it as an ELF file and ran it. Here’s what I did:
1. Swapped the key functions to ensure they matched the correct order.
2. Patched the memcmp() check with NOPs to bypass the error message.
<br>
<p align="center">
  <img src="https://i.imgflip.com/97ajar.jpg" title="The Flag!!!" width="90%" />
</p>


#### Solving with GDB:

##### Step 1: Calculate the Relative Offsets
In x86-64, the call instruction uses relative addressing, meaning it stores a signed 32-bit offset from the current instruction to the target function. To calculate this offset, you need to subtract the address of the current instruction (plus 5 to account for the size of the call instruction) from the address of the function you're calling.

To calculate the relative offset for the_third_key and the_first_key, I used the following commands in GDB:
```bash
(gdb) p/x (int)((long)the_third_key - (long)(0x55555555537c + 5))
$9 = 0xfffffe7c  # Relative offset to call the_third_key
```
```bash
(gdb) p/x (int)((long)the_first_key - (long)(0x555555555302 + 5))
$10 = 0xfffffe8e  # Relative offset to call the_first_key

```
<br> 

##### In this case:
* The calculated relative offset to call the_third_key is 0xfffffe7c.
* The calculated relative offset to call the_first_key is 0xfffffe8e.

##### Step 2: Swap the Function Calls
Now that we have the correct relative offsets, we need to patch the call instructions. This involves replacing the old offsets with the new ones.
* The call instruction at address 0x555555555302 originally called the_third_key. We’ll change it to call the_first_key by setting the new relative offset (0xfffffe8e):
```bash
(gdb) set *(int*)(0x555555555303) = 0xfffffe8e
```
* The call instruction at address 0x55555555537c originally called the_first_key. We’ll change it to call the_third_key by setting the new relative offset (0xfffffe7c):
```bash
(gdb) set *(int*)(0x55555555537d) = 0xfffffe7c
```

##### We can verify the chnages with:
```
(gdb) disassemble 0x555555555302
(gdb) disassemble 0x55555555537c
```
##### After verifying, we continue through the program execution and get the flag:
```
(gdb) c
Continuing.
[*] Insert the three keys to claim your prize!
[*] Just be careful to insert them in the right order...
[*] Your prize is: HTB{l3t_th3_hun7_b3g1n!}
[Inferior 1 (process 211367) exited normally]
```
