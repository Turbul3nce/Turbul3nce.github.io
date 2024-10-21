---
layout: ctf-layout
title: Graverobber
permalink: /writeups/reversing/graverobber
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/97ajfg.jpg" title="Graverobber" width="90%" />
</p>

##### Difficulty: <b>Very Easy</b> 

##### Description:
We're breaking into the catacombs to find a rumoured great treasure - I hope there's no vengeful spirits down there...

##### Challenge: 
The binary provided for the challenge is a simple program that checks whether a certain "path" (or string) exists using the stat() function. If the path does not exist, it prints an error message. However, if the check succeeds, the program prints a message saying "We found the treasure!", indicating the end of the program.

##### Analyzing the Binary with Ghidra
After loading the binary into Ghidra, I started by looking at the main function to understand its logic.
* The program initializes an array (local_58) to store a string, potentially the flag.
* A loop runs 32 times (0x1f in hex is 31 in decimal), constructing the string from a memory location (parts).
* After constructing a part of the string, the program calls stat() to check if the constructed string exists. If the check fails, the program prints "We took a wrong turning!" and exits.
* If the loop completes successfully, the program prints "We found the treasure!".

It's obvious the flag is being stored in local_58 as part of the string constructed in the loop.

##### Bypassing the stat() Check
The stat() call was critical because it was stopping us from reaching the part of the program that prints the treasure message. We needed to bypass the stat() check to let the program continue building the string.

Here’s the relevant section from the assembly in Ghidra:
```asm
00101216 85 c0           TEST       EAX,EAX            # Test return value of stat()
00101218 74 16           JZ         LAB_00101230       # Jump if stat() returns 0 (success)
0010121a 48 8d 05        LEA        RAX,[s_We_took_a_wrong_turning!_00102008]
```
* The program uses TEST EAX, EAX to check the return value of stat().
* JZ (Jump if Zero) ensures that if stat() succeeds, the program continues. Otherwise, it prints the failure message.

To bypass this check, I changed the JZ (0x74) to an unconditional jump (JMP, 0xEB) in Ghidra.

After modifying the jump instruction, the program no longer checks stat() and always proceeds as if the check succeeded, allowing us to reach the "We found the treasure!" message.

##### Using GDB to Retrieve the Flag
After modifying and saving the binary, I loaded it into GDB to extract the flag. Here’s how I did it:
```
// Set a Breakpoint at puts: Since puts is used to print messages, I set a breakpoint at puts to catch the program when it prints either the treasure message or other output.
(gdb) b puts
// Run the Program: I then ran the program in GDB, and it hit the breakpoint when it reached the "We found the treasure!" message.
(gdb) run
// Examine the Registers: To find the flag, I inspected the contents of the registers. The RAX register contained the treasure message, but RSI (another register) held the actual flag data.
(gdb) x/s $rax  # Check the RAX register (treasure message)
(gdb) x/s $rsi  # Check the RSI register (flag data)
0x7fffffffdc80: "H/T/B/{/b/r/3/4/k/1/n/9/_/d/0/w/n/_/t/h/3/_/s/y/s/c/4/l/l/5/}/"
```


