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
Another straightforward web challenge from the Fall Huddle. The application has a Local File Inclusion (LFI) vulnerability in the id parameter. User input directly determines which file gets included in the response, such as 1.php, 2.php, etc. The application defaults to the "php" extension, but since there’s no validation, we can override it by adding the ext parameter to our request. This allows us to include sensitive files like flag.txt in the server response. To take things a step further, I also demonstrate gaining Remote Code Execution (RCE) by exploiting log poisoning, because, when possible, I like to get a shell!

##### Vulnerable Code:
In the potato method, user-controlled input (id and ext parameters from the query string) are directly used to construct the file path for inclusion. Since no validation or sanitization is applied, we can manipulate these parameters (e.g., with ../../../etc/passwd as the id and txt and the ext) to include arbitrary files on the server.
###### IndexController.php file:
```php
<?php

class IndexController
{
    public function index($router)
    {
        $directory = __DIR__."/../views/potatoes";
        $filesAndDirs = scandir($directory);
        $filenames = [];

        foreach ($filesAndDirs as $file) {
            $fileInfo = pathinfo($file);

            if (!is_dir($directory."/".$file)) {
                $filenames[] = $fileInfo["filename"];
            }
        }

        $router->view("index", ["filenames" => $filenames]);
    }

    public function potato($router)
    {
        $id = isset($_GET["id"]) ? $_GET["id"] : "1";
        $ext = isset($_GET["ext"]) ? $_GET["ext"] : "php";
        include __DIR__."/../views/potatoes/".$id.".".$ext;
        exit;
    }
}
```
<br>

##### Solution:
Once we navigate to the application using our browser, we see three potatos. Each of them gets loaded using the ID parameter.
<p align="center">
  <img src="https://i.imgflip.com/972gx1.jpg" title="ID Parameter" width="85%" />
</p>
<br>

At first, I couldn’t include any files beyond the default ones provided by the application. I suspected something in the way the id parameter was being handled, so I checked the source code. A quick look at the index controller revealed why I couldn't get other files to load. There was another hidden paramter "ext", which was set as 'php' by default. I realized that by manually adding the ext parameter to the URL, I could specify any file extension. Sure enough, after including the ext parameter, I was able to access other files, including the flag:
<p align="center">
  <img src="https://i.imgflip.com/972jfn.jpg" title="The Flag" width="95%" />
</p>
<br>
<br>

##### RCE via Log Poisoning:
After grabbing the flag, I decided to take it a step further and try to get a shell. Since there was an LFI vulnerability, I realized that log poisoning would be a simple way to escalate this to remote code execution. I verified I could include the nginx access.log file. I then captured a request to the server, modified the User-Agent header to contain a PHP reverse shell, and forwarded the request.
<p align="center">
  <img src="https://i.imgflip.com/972ips.jpg" title="Burp Request" width="85%" />
</p>
<br>

With a netcat listener set up on port 1234, I was able to establish a shell connection.
<p align="center">
  <img src="https://i.imgflip.com/972j7z.jpg" title="RCE" width="85%" />
</p>
