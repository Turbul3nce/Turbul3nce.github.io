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


#### ASN Identification:
   - **Manual Method:** Utilize [BGP HE](http://bgp.he.net) to find Autonomous System Numbers (ASNs).
   - **Automated Tools:**
     - **ASNLookup:** A straightforward ASN lookup utility.
     - **Amass Intel:** Use with `asn` flag for detailed ASN information (`amass intel -asn [ASN_NUMBER]`).

#### Root Domain Discovery:
   - **Reverse WHOIS:** Automate using DOMLink for domain linking based on WHOIS data.
   - **Google Dorks:** Utilize advanced Google search operators to find target domains.
   - **Shodan:** Employ Shodan for uncovering exposed services linked to your target.

#### Subdomain Enumeration

##### Linked and JS Discovery:
   - **Objective:** Identify all links and embedded JavaScript within client-side code.
   - **Tool:** 
     - **Burp Suite:** Grab useful extensions for passive discovery: JS Miner. Manual review.

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

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)

[Back to top](#navigation)

---
