---
layout: ctf-layout
title: Game Capsule
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
This was likely the easiest web challenge from the Fall Huddle CTF. It involved a straightforward Local File Inclusion (LFI) vulnerability. The main issue was found in the index.php file, which had a feature allowing users to search for books on the server by providing input through the book parameter. Fortunately, the application didn't sanitize the input properly. As a result, our input was directly used to construct the file path, which meant we could manipulate the book parameter to include arbitrary files from the server. This allowed us to retrieve sensitive files easily. Could go down the RCE path for this one as well, but didn't since I showed that in the Easy Challenge: 'PotatoHealthProgram'

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
Very simple. Insert LFI payload into the book parameter and retrieve the flag. If desired, we could escalate to RCE via log poisoning.


<p align="center">
  <img src="https://i.imgflip.com/972x21.jpg" title="The Flag" width="90%" />
</p>
<br>
