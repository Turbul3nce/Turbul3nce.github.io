---
layout: ctf-layout
title: Bibliophile
permalink: /writeups/web/bibliophile
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/972xdb.jpg" title="Bibliophile" width="100%" />
</p>

##### Difficulty: <b>Medium</b> 

##### Description:
Navigate through the library's file system to uncover a hidden manuscript rumored to contain a powerful secret. Use your skills to explore beyond the intended access points.

##### Challenge: 
This was one of the easier web challenges from the Fall Huddle CTF. It involved a straightforward Local File Inclusion (LFI) vulnerability. The main issue was found in the index.php file, which allowed users to search for books on the server by providing input through the book parameter. The application doesn't sanitize the input at all. As a result, our input was directly used to construct the file path, which meant we could easily manipulate the book parameter to include arbitrary files from the server. This allowed us to retrieve the flag fairly quick.

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
Very simple. Insert LFI payload into the book parameter and retrieve the flag.


<p align="center">
  <img src="https://i.imgflip.com/972x21.jpg" title="The Flag" width="90%" />
</p>
<br>
