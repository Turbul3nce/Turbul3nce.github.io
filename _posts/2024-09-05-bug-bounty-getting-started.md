---
layout: post
title:  "Bug Hunting: Getting Started"
author: chandler
categories: [ bug bounty,]
image: assets/images/nervous-hacker.png
featured: true
---

## Echo "Hello $Hacker" | ./initiate_bugbounty 

Hello to the two or three people out there who read these posts. I'm pretty sure I write these more for myself than anyone else. It's a great way to track my progress, and I get some creative writing in—win-win ;) Anyway, let's get down to business. I wanted to take some time to write a short post for aspiring bug bounty hunters out there. 

As someone who recently got into bug hunting, and with the field growing so rapidly, maybe this will help a few newcomers. My credentials, you ask? What got me into bug bounty hunting in the first place was completing the [Bug Bounty Path](https://academy.hackthebox.com/preview/certifications/htb-certified-bug-bounty-hunter) from Hack The Box. By the way, great course! I fully recommend it for any aspiring pentesters and bug bounty hunters. 

After finishing the course, I started my bug bounty journey on HackerOne. Two weekends and a bunch of hacking away on VDP programs (I do have a full-time job), I had submitted 10 bugs, with 8 actually triaged. During this time, I also discovered my first zero-day vulnerability (I'll write a whole other post about that—and the joy, or lack thereof, of talking with vendors) in a COTS application. (Worth a level up? Possibly.) To be fair, this commercial application was part of a target with a `HUGE` scope, and so I didn't need to dedicate as much time to research as I would with other products. But enough about me.
<p align="center">
  <img src="https://i.imgflip.com/92oh7v.jpg" alt="Confused" title="Where's The Bounty" width="80%" />
</p>

## What to Know?

Let’s pretend you’re not a total newbie. Maybe you’ve got a few hacker certifications under your belt, or perhaps you're already in the field with some experience, but you’re just not sure where to start with bug bounty hunting. It’s important to understand some key differences with bug bounty hunting:

1. This isn’t a pentest, but it should be treated like one. By that, I mean: Be thorough! Log everything and take notes on everything. I’d say this is even more critical in bug bounty hunting than in pentesting or CTFs.

2. There’s nothing quick about bug bounty hunting. You don’t have 40 hours to hack this target—you’re in it for the long game, similar to what real threat actors face. Take your time. You could spend weeks or even months on a single target, depending on the scope.

3. There’s a bit of luck involved. No matter how skilled you are, someone else may beat you to a finding.

4. Approach it with an open mindset and a solid methodology that you can build upon as you hunt.

5. Bug bounty hunting is like an Easter egg hunt! Not all targets are vulnerable, and you may spend an ungodly amount of time on one target only to find absolutely nothing. And that’s okay, because your next target may have a bug around every corner, making it all worth it.

## Where to start?

There are plenty of solid public programs to start hacking on. [HackerOne](https://www.hackerone.com/), [BugCrowd](https://www.bugcrowd.com/), [Integriti](https://www.intigriti.com/), etc.There are also private programs you can apply for, like [Synack](https://www.synack.com/).With these in mind, choose a program, sign up, and move on to the next step: selecting a target! Arguably, one of the most important parts of starting in bug bounty is choosing your target. As a beginner, you’ll want to select a target appropriate for your experience level. Starting with a target offering rewards from $10k to $200k likely won’t be a very fruitful hacking experience for you.

This is where I’d suggest starting with a Vulnerability Disclosure Program (VDP) (cue eye rolls and echoes of “free labor?!"). Of course, this is optional, and it comes down to a question you need to ask yourself: Why are you doing this? Are you here to make money? Improve your resume and skills? Or maybe just for fun? Either way, it’s up to you. I’m just offering the easiest path to finding your first bug. 

That said, VDP programs generally have fewer hackers participating, which increases your chances of finding bugs. They’ll also help you get your feet wet and become more comfortable hunting on targets and sticking to a scope. 

The final factor when selecting your target is the scope. This is something you definitely want to take your time with and consider carefully. Ideally, you'll want to choose a target with a wide attack surface. What does that look like, you ask? Well, generally, it means a target with plenty of wildcards:
<p align="center">
  <img src="../assets/images/bug_bounty_scope.png" alt="Burp-Intruder" title="Burp Intruder Settings" width="80%" />
</p>
<br>
Furthermore, you’ll want to review the details of a target to see what types of vulnerabilities are in scope. If the target has a long list of vulnerabilities they don’t accept, it might not be the most beginner-friendly program. Another factor to consider is how many vulnerabilities have been submitted for that target recently. You’ll have better luck with a target that has had 500 bugs submitted in the last 90 days than one that has had just 10.

Here’s a quick lead on a target I always recommend to people just getting started: the Department of Defense has a bug bounty program through HackerOne. Their scope is massive—pretty much any .mil domain is in scope—and they generally accept bugs that other programs might not. Just to give you an idea of how "beginner-friendly" this target is based on statistics, in the last 90 days, their program has received over 1,700 bug submissions. Crazy, right? But a fantastic opportunity for newcomers nonetheless.

Now that you’ve found a suitable target, we can get to the good stuff!

## Let's Hack

In this section, I want to talk about hunting and methodology. I won’t get too technical—there are plenty of other resources for that (including this website). What I think is important here is quickly understanding recon and focusing on one or two specific vulnerabilities. During recon, your goal should be to gather as many subdomains and open ports from the targets in scope. 

[Project discovery](https://github.com/projectdiscovery) has all the tools you’ll need for this process, with a few exceptions, but it’s a great place to start for recon tooling. Once you’ve built a solid methodology for recon, you should focus on one or two vulnerabilities to become very familiar with. 

People generally start with vulnerabilities like XSS, information disclosures, or IDORs. It doesn’t matter which ones you choose—what’s important is that you understand them thoroughly. Do as many PortSwigger labs as you can. Take all the notes! Build scripts and Nuclei templates to automate the search for these vulnerabilities. Understand the vulnerabilities so deeply that you can almost anticipate what the developer was thinking when they introduced the bug into the code. Again, PortSwigger is an excellent resource for gaining this level of knowledge. Do the labs and take detailed notes. 

Another worthwhile step, though not necessary, would be to learn some frontend or backend programming (depending on the vulnerabilities you’re hunting). You can use free resources like the [Odin Project](https://www.theodinproject.com) to get familiar with coding practices, and it certainly doesn’t hurt to know how something works when you’re trying to break it!
<p align="center">
  <img src="../assets/images/odin-project.png" alt="odin-project" title="The Odin Project" width="80%" />
</p>

And with that, I’ll leave you to it. Go out and hack all the things. Remember, you are representing yourself and your future in the security community. Act ethically and <b>always</b> <b>always</b> stay in scope. Feel free to check out more of my blogs on topics like OSINT, the importance of fuzzing to expand your attack surface, and how I got into Synack! Hack the planet!
