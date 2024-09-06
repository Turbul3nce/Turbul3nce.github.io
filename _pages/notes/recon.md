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
