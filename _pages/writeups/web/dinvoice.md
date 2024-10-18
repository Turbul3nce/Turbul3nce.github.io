---
layout: ctf-layout
title: dinvoice
permalink: /writeups/web/dinvoice
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/972xdb.jpg" title="Bibliophile" width="100%" />
</p>

##### Difficulty: <b>Medium</b> 

##### Description:
Our newest web service, "dInvoice", needs an inspection from you to unveil any critical vulnerabilities it might have that can lead to the compromise of the admin account!

##### Challenge: 
This was one of the more interesting challenges from the Fall Huddle. There are several possible solutions. Funny enough, we could only get the two unintended solutions. The first solution our team found was with a directory travseral within the the file path of where the user's markdown files are stored. The secdon unintended way to solve the challenge involved the way the JWT SECRET was created. It was a random integer that could easily be bruteforced.

##### Vulnerable Code:
The $book_name is set directly from the $_GET['book'] input and is used in the file_exists() and include() functions without proper validation or sanitization.
###### Index.php:
```php
$book_name = htmlspecialchars($_GET['book']);
$file_path = $book_name;

if (file_exists($file_path)) {
    include($file_path);
}
```
<br>

##### Solution:
The application allows us to register an

<p align="center">
  <img src="https://i.imgflip.com/972x21.jpg" title="The Flag" width="90%" />
</p>
<br>
