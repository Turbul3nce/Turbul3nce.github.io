---
layout: ctf-layout
title: Chandler Rose
permalink: /writeups/web/game-capsule
comments: false
---

### Echo $Hacker | ./initiate_bugbounty 

Hello to the two or three people out there who read these posts. I’m pretty sure I write these more for myself than anyone else. It’s a great way to track my progress and get some writing in—win-win! Anyway, let’s get down to business. 

As a recent entrant to the bug bounty scene with moderate success, I hope to share a short guide to help those who find themselves where I once did. Wondering about my credentials? Well, what originally got me into bug bounty hunting was completing the [Bug Bounty Path](https://academy.hackthebox.com/preview/certifications/htb-certified-bug-bounty-hunter) from Hack The Box. By the way, great course! I fully recommend it for any aspiring pentesters and bug bounty hunters. 

After finishing the course, I started my bug bounty journey on HackerOne. Two weekends and a lot of hacking on VDP programs (I know, I know), I submitted 10 bugs, 8 of which were triaged. During that time, I discovered my first zero-day vulnerability (I'll save the joys—and frustrations—of dealing with vendors for another post) in a COTS application. To be fair, the target had a `HUGE` scope and likely hadn’t been tested much, so the research time probably wasn’t as intense as it should have been. But enough about me.
<p align="center">
  <img src="https://i.imgflip.com/92oh7v.jpg" alt="Confused" title="Where's The Bounty" width="80%" />
</p>

### What to Know?

Let’s pretend you’re not a total newbie. Maybe you’ve got some hacker certifications or field experience, but you’re unsure where to start with bug bounty hunting. It's crucial to understand some key differences:

1. This isn’t a pentest, but treat it like one. Be thorough! Log and document everything—this is even more critical in bug bounty hunting than in pentests or CTFs.

2. Bug bounty hunting isn’t fast. You’re in it for the long haul, much like real threat actors. Take your time—one target could take weeks or months depending on its scope.

3. Luck plays a role. No matter how skilled you are, someone else might beat you to a bug.

4. Keep an open mindset and build upon a solid methodology as you hunt.

5. Bug bounty hunting is like an Easter egg hunt! You may spend a lot of time on a target and find nothing—that’s okay. Your next target may be full of bugs, making it all worth it.

### Where to Start?

There are plenty of solid public programs to start hacking on. [HackerOne](https://www.hackerone.com/), [BugCrowd](https://www.bugcrowd.com/), [Integriti](https://www.intigriti.com/), etc. There are also private programs you can apply for, like [Synack](https://www.synack.com/). With that in mind, choose a program, sign up, and move on to the next step: selecting a target! One of the most important parts of starting in bug bounty is choosing a target appropriate for your experience level. As a beginner, starting with a target offering rewards from $10k to $200k likely won’t lead to a very fruitful hacking experience.

This is where I’d suggest starting with a Vulnerability Disclosure Program (VDP) (cue the eye rolls and echoes of ‘free labor?!’). Of course, this is optional, and it comes down to one question: Why are you doing this? Are you here to make money? Improve your resume and skills? Or maybe just for fun? Either way, it’s up to you—I’m just offering the easiest path to finding your first bug.

VDP programs typically have fewer participants, increasing your chances of finding bugs. They’ll help you get comfortable hunting and sticking to a scope.

When selecting your target, the final factor to consider is the scope, and it’s something you’ll want to approach carefully. Ideally, choose a target with a wide attack surface. What does that look like? Generally, I look for wildcards—the more, the better! This means all subdomains of the root domain are in scope, allowing your recon skills to shine. If you’re able to find hidden subdomains that haven’t been tested by other hackers, it increases your chances of finding bugs.
<p align="center">
  <img src="../assets/images/bug_bounty_scope.png" alt="Burp-Intruder" title="Burp Intruder Settings" width="80%" />
</p>
