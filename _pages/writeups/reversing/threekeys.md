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
The objective was pretty clear: find out the correct order of the keys and decrypt the hidden flag.

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
