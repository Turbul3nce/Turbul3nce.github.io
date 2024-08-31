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
<h5>Buy me a coffee</h5>

<p>Thank you for your support!</p>

<a target="_blank" href="https://buymeacoffee.com/rosehacksls" class="btn btn-danger">Buy me a coffee</a>

</div>
</div>
</div>

## Navigation

- [Recon Methodology](#Recon)
- [Application Checklist](#app-checklist)
- [Vulnerability Testing](#vulnerability-testing)
- [References](#references)

---

## Recon

#### Core Objectives:
- **Identify all subdomains!**
- **Identify all open ports!**

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


[Back to top](#navigation)

---

## Application Checklist - Getting Started

1. What technology stack and programming languages are used in the application?
2. What is the intended use of the application?
3. What type of server or hosting environment is powering the application?
4. Is there a Web Application Firewall (WAF) in place?
5. What third-party libraries and frameworks are integrated? Are there known vulnerabilities associated with these?
6. What authentication mechanisms are implemented?
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


[Back to top](#navigation)

---

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)

[Back to top](#navigation)

---
