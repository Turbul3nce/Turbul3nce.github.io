---
layout: ctf-layout
title: Game Capsule
permalink: /writeups/web/game-capsule
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/96zbvn.jpg" title="Game Capsule" width="100%" />
</p>

##### Difficulty: <b>Easy</b> 

##### Description:
A popular game-selling app is secretly offering a blacklisted, highly coveted game hidden from public view. Your mission is to hack into the app’s backend, bypass its security, and uncover the hidden game to add it to your collection. Can you outsmart the system and get the forbidden game in your hands?

##### Challenge: 
This was a fairly easy web challenge that required about 2 minutes of my time. The NodeJS application uses JWT to assign/define user roles. It renders the page differently depending on the username inside the JWT. So, I was able to forge a JWT for any user, in this case the "admin" user, which the application uses to determine whether the flag should read from /flag.txt and rendered onto the index.html page.

##### Vulnerable Code:
Inside the AuthMiddleWare file there are two functions, the setcookie and the getcookie function. The getcookie function checks if the incoming request has a cookie, if not the setcookie function generates one with a randomly generated username, such as "guest_ewiufhu4h98hf". It then signs the cookie. 
###### AuthMiddleWare.js file:
<p align="center">
  <img src="https://i.imgflip.com/971zlf.jpg" title="AuthMiddleWare" width="100%" />
</p>
<br>

The routes file gets the cookie from the AuthMiddleWare and checks the value of the "username", if it is equal to admin it reads the flag from /flag.txt and renders it on the index.html page, if the value is anything else, it just loads the index.html page without the flag. 
###### Routes.js file:
<p align="center">
  <img src="https://i.imgflip.com/972210.jpg" title="routes.js" width="100%" />
</p>
<br> 

##### Solution:
After landing on the application, we notice there isn't much in terms of functionality. Just a static page. We can click through the games. That's about it. So, with that, one of the first things I checked, was the esistence of a cookie using the developer tools. An sure enough there was a JWT sitting in storage. 

<p align="center">
  <img src="https://i.imgflip.com/971yh6.jpg" title="Cookie" width="100%" />
</p>
<br>

I copied the JWT and pasted it into JWT.io to analyze it. After seeing that the application uses the JWT to determine the "username", I tried changing mine from guest_088u98u3498u89 to "admin" to see if the application would render the home page differently. 

<p align="center">
  <img src="https://i.imgflip.com/971z2r.jpg" title="JWT.io" width="100%" />
</p>
<br>

After copying the newly forged JWT, I pasted it into the storage and refreshed the page: 

<p align="center">
  <img src="https://i.imgflip.com/971z7l.jpg" title="The Flag" width="100%" />
</p>







