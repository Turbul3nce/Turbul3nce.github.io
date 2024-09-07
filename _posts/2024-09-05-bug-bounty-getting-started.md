---
layout: post
title:  "Bug Hunting: Getting Started"
author: chandler
categories: [ bug bounty,]
image: assets/images/nervous-hacker.png
featured: true
---

## Echo "Hello $Hacker" | ./initiate_bugbounty 

Hello to the two or three people out there who read my posts. Pretty sure I write these for myself more than anything. It's an easy way to track progress, and I get some creative writing in-Win Win :). Anyway, let's get to the business. I wanted to take some time to write a shortish post for those aspiring bug bounty hunters out there. As someone who recently got into bug hunting myself, and seeing as how the field is growing so rapidly, maybe this will help out a few newcomers. My credentials you ask? Well, what got me into bug bounty hunting in the first place was actually completing the [Bug Bounty Path](https://academy.hackthebox.com/preview/certifications/htb-certified-bug-bounty-hunter). By the way, great course for any aspiring penetesters and bug bounty hunters. After completing the course, I started my bug bounty journey on HackerOne. Two weekends and a bunch of hacking away on VDP programs (I do have a full time job), I had submitted 10 bugs and had 8 actually triaged. During this time, I also discovered my first zero day vulnerability (I'll write a whole other post about that, and the joy of talking with vendors or.. lack thereof.) on a COTS application (Worth a level up? possibly). Although, to be fair, this commerical application was a part of a target with a `HUGE` scope. So, I didn't really need to dedicate a ton of time into the research like some others. But enough about me. 
<p align="center">
  <img src="https://i.imgflip.com/92oh7v.jpg" alt="Confused" title="Where's The Bounty" width="80%" />
</p>

## What to Know?

Let's pretend you aren't a total newbie. Say you have few hacker certifications under your belt, or maybe you're even in the field and have some experience, but you're just not sure where to start with bug bounty. It's important to understand some key differences associated with bug bounty hunting: 
1. This isn't a pentest, but should be treated like one. And by that, I mean: Be through! Log everything and take notes on everything. I'd say even more so with bug bounty hunting than pentesting or CTFs.
2. There is nothing quick about bug bounty hunting. You don't have 40 hours to hack this target. You are in it for the long game. A position real threat actors find themsevles. Take your time. You can spend weeks or even months on a single target, depending on the scope of course.
3. There is a bit of luck involved. No matter how great of a hacker you may be, someone else may just beat you to a finding.
4. Go into it with an open mindset and solid methodolgy that you can build upon as you hunt.
5. Bug bounty is an Easter egg hunt! Not all targets are vulnerable, and you may spend ungodly amounts of time on a single target just to find absoluelty nothing. And that's okay because your next target may have a bug around every corner that makes it all worth it.  

## Where to start?

There are plenty of solid public prgrams to start hacking on out there. [HackerOne](https://www.hackerone.com/), [BugCrowd](https://www.bugcrowd.com/), [Integriti](https://www.intigriti.com/), etc. There are also private programs that you can apply for like [Synack](https://www.synack.com/). With these in mind, choose a program, sign up and move onto the next step: slecting a target! Probably one of the most important parts of starting in bug bounty is choosing you target. As a beginner, you will want to choose a target appropiate for your experience level. Starting on a target that offers a minimum 10k reward up to 200k rewards probably will not be a very frutiful hacking experince for you. This is where I would suggest Vulnerability Disclosure Program (VDP) (Cue eye rolls and echos of "free labor?!"). This is optional of course and comes down to a question you must ask yourself: Why am I doing this? Are you doing this to make money? Improve your resume and skills? Or maybe jsut for fun? Either way, this is up to you. I am just offereing the easiest path to finding to finding your first bug. Anywho, these VDP programs will generally have less hackers on them, increasing your chances of finding your first bug(s). This will also help get your feet wet, and get you more comfortable hunting on targets and adhering to a scope. The final factor when selecting your target will be the `Scope`. Something you defintley want to take you time with and consider carefully. Here, you will want to choose a target with a wide attack surface. What will this look like you ask? Well, generally you will want to see a target that has a bunch of wildcards: 
<p align="center">
  <img src="../assets/images/bug_bounty_scope.png" alt="Burp-Intruder" title="Burp Intruder Settings" width="80%" />
</p>
<br>
Furthermore, you'll want to check out the details on a target to see what sort of vulnerabiltiies are in scope. If the target had a huge list of vulnerabilties they do not accept, maybe this isn't a very beginner friendly program. Now that you have finally found a suitable target, we can get to the good stuff!

## Let's Hack

In this section, I want to discuss hunting and methodology. I want get that technical. There's plenty of other resources out there for that (including this website). What I think is important here is to quickly understand the recon and focusing on one or two specific vulnerabilties. During recon, your goal should be to gather as many subdomains and open ports from the targets in scope. [Project discovery](https://github.com/projectdiscovery) has all the tools you will need for this process with a few exceptions, but it is a great place to start for recon tooling. Once you've built a solid methodology for recon, you should choose one or two vulnerabilities to become very familiar with. People generally start with vulnerabilities, such as XSS, information disclosures, IDORs, etc. It really doesn't matter which ones you choose. What matters is that you understand them through and through. Do as many POrtswigger labs as you can. Take all the notes! Build out scripts and nuclei templates that automate search for them. Understand the ins and outs. You should know what the developer was thinking when he introduced the bug into the code. Again, `Portswigger` is a great place to get this level of knowledge. Do the labs and take the notes. Another step worth taking, although not necessary, would be to learn some frontend or backend programming (depending on the vulnerabiltiies you're hunting). You can use free resources like the [Odin Project](https://www.theodinproject.com) to get familiar with coding practices and it certainly doesn't hurt to know how something works when trying to break it!

Post about how to get into bug bounty as a complete beginner. I started Bug bounty a week ago and within that time, I have already racked up several several bugs that have been triaged. Why are you doing bug bounty? for money, to improve your resume job search, etc? Know your tools, proper training, oppertunities on Hackerone (bounty and VDP), choosing scope and expanding attack surface. targeting specififc application and where to go from there.
