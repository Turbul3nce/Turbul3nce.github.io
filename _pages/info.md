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

<h2>Navigation</h2>
<ul>
    <li><a href="#information-gathering">Recon Methodology</a></li>
    <li><a href="#application-checklist">Application Checklist</a></li>
    <li><a href="#vulnerability-testing">Vulnerability Testing</a></li>
    <li><a href="#references">References</a></li>
</ul>

</div>
</div>
</div>

---




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
23. Are there any gadgets we can pair with vulnerabilities to show impact?



[Back to top](#navigation)

---

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)
<br>
[R-s0n GitHub](https://github.com/R-s0n)
[Back to top](#navigation)

---
