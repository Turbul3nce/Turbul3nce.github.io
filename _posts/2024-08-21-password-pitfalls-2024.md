---
layout: post
title:  "Password Pitfalls: Even in 2024, We're Still Our Own Worst Enemy"
author: chandler
categories: [ pentests,]
image: assets/images/1.jpg
featured: true
---

Earlier this year, I was conducting a penetration test for a smaller bank, which we'll leave unnamed. The test involved auditing their local infrastructure, exploring what an attacker could achieve with close access, already on the same network without any credentials to the Active Directory environment.

My initial approach included running routine attack vectors for a local intruder: scanning with `nmap`, leveraging `responder`, packet sniffing with `wireshark`, testing printers/MFPs, and executing scans with `CrackMapExec`. Despite my efforts, nothing was yielding significant results. The printers did show a few vulnerabilities, but nothing that allowed for privilege escalation. It seemed all the proper mitigations to prevent relay attacks were firmly in place, such as disabling NTLMv1, enforcing SMB signing, and implementing strict account lockout policies. This level of security was to be expected as the bank had undergone several penetration tests in the past.

After a few days of hitting dead ends, I decided to switch tactics and turned to password spraying. The bank had a mobile application listed on the app store, which, interestingly, had only four comments—all praising how "amazing" it was. I surmised these comments were likely from employees, especially since they were all posted around the same time a few months ago. This small detail is crucial because it highlights the importance of thorough OSINT—always dig deep.

With a list of names gathered from LinkedIn and the bank's website, I began password spraying using common business passwords:

```bash
crackmapexec smb 10.10.10.0/24 -u users.txt -p passwords.txt --continue-on-success
```

However, I didn't get anywhere with SMB—possibly due to some issues with crackmapexec not working at the time with the winrm service. So, I switched to using winrm-brute for password spraying:

```bash
winrm-brute --hosts 10.10.10.0/24 --usernames users.txt --passwords common_passwords.txt
```

And wouldn't you know it, Season2024! was the winning ticket—not just into any user account, but into a Domain Admin's account.

This penetration test serves as a stark reminder of how simple human errors, like predictable passwords, can still be easy wins for attackers even today. It underscores the critical need for enforcing strong password policies, continuous education on security hygiene, and regular auditing to catch these oversights before they become liabilities. Remember, the chain is only as strong as its weakest link.
