---
layout: ctf-layout
title: PotatoHealthProgram
permalink: /writeups/web/php
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/9727sa.jpg" title="PHP" width="100%" />
</p>

##### Difficulty: <b>Easy</b> 

##### Description:
You are tasked to fetch critical potato intelligence by abusing P.H.P, a system designed to securely monitor potato farm info.

##### Challenge: 
Another really easy web challenge from the Fall Huddle. The application has a Local File Inclusion (LFI) in an ID paramter. It allows user input to directly determine which file will be included in the response, such as 1.php, 2.ph, 3.php, etc. The application appends the default file extension in the code as "php", but there is no validation, so we can define our own ext by adding the ext parameter in our request. This let me include the contents of flag.txt file in the server response. I like to try to get a shell when I can, so I also demonstrate getting RCE through log poisoning.

##### Vulnerable Code:
In the potato method, user-controlled input (id and ext parameters from the query string) are directly used to construct the file path for inclusion. Since no validation or sanitization is applied, we can manipulate these parameters (e.g., with ../../../etc/passwd as the id and txt and the ext) to include arbitrary files on the server.
###### IndexController.php file:
<p align="center">
  <img src="https://i.imgflip.com/972d30.jpg" title="PHP Controller" width="100%" />
</p>
<br>

##### Solution:
Once we navigate to the application using our browser, we see three potatos. Each of them gets loaded using the ID parameter.
<p align="center">
  <img src="https://i.imgflip.com/972gx1.jpg" title="ID Parameter" width="100%" />
</p>
<br>

At first, I was unable to include any other files in the servers response. I wanted to see how the application was handding the user input in the ID parameter, so I turned to the source code. A quick look at the index controller revealed why I couldn't get other files to load. There was another hidden paramter "ext" which was set as php by default. So I thouhgt, in theory, I should just be able to just add the ext paramter to the URL to access other non php files. Sure enough, I was able to access the flag: 
<p align="center">
  <img src="https://i.imgflip.com/972jfn.jpg" title="The Flag" width="100%" />
</p>

##### RCE via Log Poisoning:
I able to grab the flag, but I like to also get a shell when possible. I figured if there was an LFI, log poisoning would be a quick and easy way to escalate it to remote code execution. So I captured a request to the sever, replaced the User Agent with a PHP reverse shell, and forwarded the request to the server. I then setup a netcat listener.
<p align="center">
  <img src="https://i.imgflip.com/972ips.jpg" title="Burp Request" width="100%" />
</p>
<br>

After including the the access.log file in the server's response, I received a connection on my netcat listener as www:
<p align="center">
  <img src="https://i.imgflip.com/972j7z.jpg" title="RCE" width="100%" />
</p>
