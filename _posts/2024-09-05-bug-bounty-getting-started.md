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
  <a href="https://imgflip.com/i/92oh7v"><img src="https://i.imgflip.com/92oh7v.jpg" title="made at imgflip.com"/></a><div><a href="https://imgflip.com/memegenerator">from Imgflip Meme Generator</a></div> width="80%" />
</p>

## What to Know?

Let's pretend you aren't a total newbie. Say you have few hacker certifications under your belt, or maybe you're even in the field and have some experience, but you're just not sure where to start with bug bounty. It's important to understand some key differences associated with bug bounty hunting: 
1. This isn't a pentest, but should be treated like one. And by that, I mean: Be through! Log everything and take notes on everything. I'd say even more so with bug bounty hunting than pentesting or CTFs.
2. There is nothing quick about bug bounty hunting. You don't have 40 hours to hack this target. You are in it for the long game. A position real threat actors find themsevles. Take your time. You can spend weeks or even months on a single target, depending on the scope of course.
3. There is a bit of luck involved. No matter how great of a hacker you may be, someone else may just beat you to a finding.
4. Go into it with an open mindset and solid methodolgy that you can build upon as you hunt.
5. Bug bounty is an Easter egg hunt! Not all targets are vulnerable, and you may spend ungodly amounts of time on a single target just to find absoluelty nothing. And that's okay because your next target may have a bug around every corner that makes it all worth it.  

## Where to start?

There are plenty of solid public prgrams to start hacking on out there. [HackerOne](https://www.hackerone.com/), [BugCrowd](https://www.bugcrowd.com/), [Integriti](https://www.intigriti.com/), etc. There are also private programs that you can apply for like [Synack](https://www.synack.com/). With these in mind, choose a program, sign up and move onto the next step: slecting a target! Probably one of the most important steps in bug bounty is choosing you target. This is even more essential if you are a beginner. As someone just getting started in bug bounty hunting, you want to choose a target appropiate for your experience level. Starting on a target that offers minimum 10k rewards up to 200k probably will not be a very frutiful experince for you. This is where I would suggest Vulnerability Disclosure Program (Cue eye rolls and echo "free labor?!"). These program will generally have less hackers on them, increasing your chances of finding your first bug. This will help you get your feet wet, and more comfortable hunting on targets, adhering to a scope, and possibly picking up your first bug. The final most important factor when selecting your target will be the `Scope`. You will want to choose a target with a wide scope. What will this look like you ask? Well, generally you will want to see a target that has a bunch of wildcards: 

Post about how to get into bug bounty as a complete beginner. I started Bug bounty a week ago and within that time, I have already racked up several several bugs that have been triaged. Why are you doing bug bounty? for money, to improve your resume job search, etc? Know your tools, proper training, oppertunities on Hackerone (bounty and VDP), choosing scope and expanding attack surface. targeting specififc application and where to go from there.
