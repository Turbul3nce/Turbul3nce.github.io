---
layout: post
title:  "OSINT Opens Doors: Weak Passwords in 2024"
author: chandler
categories: [ pentests,]
image: assets/images/1.png
featured: true
---

### Background
Earlier this year, I was on a pentest contract for a small bank, which will remain unnamed. The scope of the test focused on auditing their local network to determine what an attacker could achieve with close network access but no initial credentials to their Active Directory.

After initiating a network scan with Nessus, I proceeded with the standard toolkit for a local penetration test—running `nmap`, leveraging `responder`, packet sniffing with `wireshark`, examining printers/MFPs, and scanning services with `CrackMapExec`. Despite these efforts, no significant vulnerabilities emerged. The printers showed some weaknesses, but nothing that could facilitate privilege escalation. This was unsurprising, as the bank had several pentests in the past.

### OSINT FTW
After several days of getting nowhere and nearing the end of the assessment, I decided it was time to target some services and spray passwords. To facilitate this attack, I needed usernames. This is when I turned to Open Source Intelligence (OSINT). From the bank's website and employees’ LinkedIn profiles, I compiled a list of potential usernames. Additionally, I discovered that the bank had an app on the App Store. The bank's mobile application had only <b>three</b> reviews, all excessively positive, likely from employees given the timing of the posts. Thus, I added these names to my usernames file. The significance of obtaining usernames from the App Store reviews will become evident later on.


### Spray and Pray
With these usernames, I attempted to password spray using common business credentials:

```code
SEASONYEAR!
SEASONYEAR!@
BUSINESSNAMEYEAR1!
EMPLOYEENAMEBIRTHYEAR!
```
The Command:

```bash
crackmapexec smb 192.168.1.0/24 -u users.txt -p passwordlist.txt --continue-on-success
```

However, I didn’t get anywhere with SMB. So, I moved onto spraying WinRM credentials, but still nothing—I think this was likely due to some issues with crackmapexec not working at the time with winrm. So, I switched to using winrm-brute for password spraying.

The Command:

```bash
winrm-brute --hosts 192.168.1.0/24 --usernames users.txt --passwords common_passwords.txt
```

After some time, I checked back on winrm-brute, and there it was—a single hit! The valid username matched one of the three users from the app store reviews for the bank’s mobile application, confirming my hunch that they were employees. This particular employee’s password followed a common corporate pattern, ‘SeasonYEAR!’, which is often used in environments requiring quarterly or semiannual password changes. During the debrief, it was revealed that their IT department had recommended this format. As a result, I gained access—and not just to any domain user, but to a Domain Admin account, with a surprisingly weak password.

<p align="center">
  <img src="../assets/images/bad-password.jfif" alt="Do better!" title="Bad Passsword" width="80%" />
</p>
<br>

This assessment reveals that passwords continue to offer easy entry points for attackers. Implementing stringent password policies and regular security training is vital. Additionally, thorough <b>OSINT</b> is critical for pentesters, as it widens the attack surface, making our target easier to breach. For us, this means a greater likelihood of identifying and exploiting these gaps, effectively tipping the scales in our favor.
