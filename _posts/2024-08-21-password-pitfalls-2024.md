---
layout: post
title:  "OSINT Unlocks the Door: The Enduring Risk of Weak Passwords"
author: chandler
categories: [ pentests,]
image: assets/images/1.jpg
featured: true
---

### Background
Earlier this year, I was contracted to conduct a penetration test for a smaller bank, which will remain unnamed. The scope of the test focused on auditing their local infrastructure to determine what an attacker could achieve with close network access but no initial credentials to their Active Directory.

After initiating a network scan with Nessus, I proceeded with the standard toolkit for a local penetration test—running `nmap`, leveraging `responder`, packet sniffing with `wireshark`, examining printers/MFPs, and scanning services with `CrackMapExec`. Despite these efforts, no significant vulnerabilities emerged. The printers showed some weaknesses, but nothing that could facilitate privilege escalation. This was unsurprising, as the bank had several mitigations in place against relay attacks and other common vulnerabilities.

### Pray and Spray 
After several days of getting nowhere, I decided it was time to target services and spray some passwords. To facilitate this attack, I needed usernames. From the bank's website and employees’ LinkedIn profiles, I compiled a list of potential usernames. Additionally, I discovered that the bank had an app on the App Store. The bank's mobile application had only four reviews, all excessively positive, likely from employees given the timing of the posts. Thus, I added these names to my usernames file. The significance of obtaining usernames from the App Store reviews becomes evident later on when the user I managed to login as belonged to one of these reviewers. This highlights the importance of thorough Open Source Intelligence (OSINT).

With these usernames, I attempted to password spray using common business credentials:

```bash
crackmapexec smb 192.168.1.0/24 -u users.txt -p passwordlist.txt --continue-on-success
```

However, I didn’t get anywhere with SMB. So, I moved onto spraying against WinRM, but still nothing—I think this was due to some issues with crackmapexec not working at the time with the winrm service. So, I switched to using winrm-brute for password spraying.

```bash
winrm-brute --hosts 10.10.10.0/24 --usernames users.txt --passwords common_passwords.txt
```

And wouldn’t you know it, there was a hit! The password was in the format of SeasonYEAR!. And just like that, I was in! But this wasn’t just any domain user, it was a Domain Admin! A domain admin with a weak password.

This penetration test underscores how predictable passwords continue to offer easy entry points for attackers. Implementing stringent password policies and regular security training is vital. Additionally, thorough OSINT is critical, as it expands the attack surface, making vulnerabilities more accessible and the target easier to breach. For pentesters, this means a greater likelihood of identifying and exploiting gaps, effectively tipping the scales in our favor.
