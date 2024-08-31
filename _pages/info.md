---
layout: page
title: Chandler Rose
permalink: /info
comments: false
---

# Resources

## Navigation

- [Methodology](#methodology)
- [App Checklist](#app-checklist)
- [Top Vulns](#top-vulns)
- [Testing Vulns](#testing-vulns)
- [Resources](#resources)

---

## Methodology

## Reconnaissance

### Core Objectives:
- **Identify all subdomains!**
- **Scan all ports for potential entry points.**

### Essential Tools for Setup:
- **MassDNS:** For brute-forcing subdomains.
- **Intrigue.io:** For comprehensive reconnaissance and information gathering.

### Custom Scripts To Do:
1. Develop a Python script to search GitHub for relevant repositories and return only those with results - **Completed!**

---

## Recon Methodology

### 1. Information Gathering:
   - **Target Details:** Collect comprehensive data on the target, including potential acquisitions.
   - **Tool:** Use [Crunchbase](https://www.crunchbase.com) to identify acquisitions related to the target.

### 2. ASN Identification:
   - **Manual Method:** Utilize [BGP HE](http://bgp.he.net) to find Autonomous System Numbers (ASNs).
   - **Automated Tools:**
     - **Metabigor:** An efficient tool for ASN reconnaissance.
     - **ASNLookup:** A straightforward ASN lookup utility.
     - **Amass Intel:** Use with `asn` flag for detailed ASN information (`amass intel -asn [ASN_NUMBER]`).

### 3. Root Domain Discovery:
   - **Reverse WHOIS:** Automate using DOMLink for domain linking based on WHOIS data.
   - **Ad/Analytics Relationships:** Explore relationships using [BuiltWith](https://builtwith.com) for Ad and Analytics identifiers.
   - **Google Dorks:** Utilize advanced Google search operators to find target domains.
   - **Shodan:** Employ Shodan for uncovering exposed services linked to your target.

### 4. Subdomain Enumeration

#### a. Linked and JS Discovery:
   - **Objective:** Identify all links and embedded JavaScript within client-side code.
   - **Tool:** 
     - **Burp Suite Pro:** Disable passive scanning initially to focus on manual discovery.

### 5. Automation Tasks:

- **Cron Jobs:**
  - **Every 6 hours:** Run `kindling.py` - **Completed!**
  - **Every 24 hours:** Run `fire_starter.py` - **Completed!**
  - **Weekly:** Run `firewood.py` - **Completed!**

- **Enhancements:**
  - Added additional ports to `kindling.py` using `httprobe` - **Completed!**
  - Valuable data on the dashboard - **Completed!**
  - Cloud Ranges Module - **Completed!**
  - Custom Wordlist Module - **Completed!**
  - Application/Server Info Modules - In Progress
  - Fix Subdomainizer Module - In Progress

#### b. Subdomain Scraping:
   - **Infrastructure Sources:** Gather data from sources like Censys, DnsDumpster, and WaybackMachine.
   - **Certificate Sources:** Explore SSL certificate registries like crt.sh, CertDB, and Cert Spotter.
   - **Search Engines:** Utilize Google, Yahoo, and Baidu for domain discovery.
   - **Security Databases:** Leverage databases like VirusTotal, Rapid7 Project Sonar, and SecurityTrails.

   - **Recommended Tools:**
     - **Amass and Subfinder:** Powerful tools for ASN enumeration and subdomain discovery.
     - **GitHub Subdomain Scraping:** Use `github-subdomains.py` with varying sleep intervals to avoid rate limiting.
     - **Shodan Parser:** Utilize `shosubgo` for parsing Shodan results.
     - **Cloud Range Monitoring:** Monitor SSL sites within AWS, GCP, and Azure ranges. Parse certificates to match with your target using tools like `tls.bufferover.run`.

#### c. Subdomain Bruteforcing:
   - **Tools:**
     - **Amass:** Command: `amass enum -brute -d [DOMAIN] -rf`
     - **ShuffleDNS:** A massDNS wrapper for effective subdomain brute-forcing.
     - **Wordlists:**
       - **Tailored Wordlists:** Generate using TomNomNom and CeWL.
       - **Massive Wordlists:** Use comprehensive lists like JHaddix's `all.txt`.
     - **Commonspeak2:** Source wordlists from GitHub.
     - **Subdomain Alterations:** Experiment with variations like `www.target.com` to `ww2.target.com`.

### 6. Port Scanning and Analysis:
   - **Masscan:** Quickly identify open ports (requires an IP list).
   - **dnMasscan:** Resolves domain names to IP addresses and passes them to Masscan.
   - **Nmap:** Perform a detailed analysis of discovered open ports.

### 7. GitHub Dorking:
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

### 8. Httprobe and Eyewitness:
   - Use **Httprobe** to find live hosts and **Eyewitness** to capture screenshots for visual inspection.

### 9. Subdomain Takeover:
   - **Resources:**
     - **Can-I-Take-Over-XYZ:** Database by EdOverflow listing subdomain takeover techniques.
     - **SubOver:** Tool to check for vulnerable subdomains.
     - **Nuclei:** Run templates to check for takeover vulnerabilities.


[Back to top](#resources)

---

## App Checklist

#### What technology stack and programming languages are used in the application?
#### What type of server or hosting environment is powering the application?
#### Is there a Web Application Firewall (WAF) in place?
#### What third-party libraries and frameworks are integrated? Are there known vulnerabilities associated with these?
#### What authentication mechanisms are implemented?
#### Are there any long redirection responses?
#### What objects or data structures are utilized within the application?
#### How are sessions created, maintained, and managed?
#### Are there any useful comments or metadata within the code?
#### How does the application handle special characters or input encoding?
#### Can specific actions or inputs trigger error messages or stack traces?
#### What common features or functionalities are included in the application?
#### How is a user uniquely identified and tracked?
#### Are there different user roles or permission levels within the application?
#### Does the application expose or utilize an API?
#### Is a Content Management System (CMS) integrated into the application?
#### Is a Content Security Policy (CSP) in place to mitigate cross-site scripting attacks?
#### Is Cross-Origin Resource Sharing (CORS) implemented, and how is it configured?
#### Is CAPTCHA or another bot detection mechanism used?
#### Are WebSockets or similar technologies employed for real-time communication?
#### Is the source code or any part of it publicly accessible?
[Back to top](#resources)

---

## Top Vulns

Discover the top vulnerabilities commonly found in web applications. This section provides an overview of these vulnerabilities, their impact, and how to identify them.

[Back to top](#bugfolio-resources)

---

## Testing Vulns

This section dives deep into the techniques used to test for specific vulnerabilities. Whether you're testing for SQL injection, XSS, or other types of vulnerabilities, you'll find detailed guides here.

[Back to top](#bugfolio-resources)

---

## Resources

Find additional resources, including tools, scripts, and references that can aid in your bug bounty hunting efforts. This section is constantly updated with the latest and most useful information.

[Back to top](#bugfolio-resources)

---
