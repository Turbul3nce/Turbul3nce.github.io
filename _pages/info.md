---
layout: page
title: Bug Bounty Guide
permalink: /info
comments: false
---

<h2>Navigation</h2>
<div>
  <table>
    <tr>
      <!-- Navigation menu on the left -->
      <td>
        <ul>
          <li><a href="/notes/recon-methodology">Recon Methodology</a></li>
          <li><a href="#application-checklist">Application Checklist</a></li>
          <li>
            <a href="#vulnerability-testing">Vulnerability Testing</a>
            <ul>
              <li><a href="#sqli">SQL Injection</a></li>
              <li><a href="#xss">Cross-Site Scripting</a></li>
              <li><a href="#host-header-injection">Host Header Injection</a></li>
              <li><a href="#cmdi">Command Injection</a></li>
              <li><a href="#improper-authentication">Improper Authentication</a></li>
              <li><a href="#csrf">Cross-Site Request Forgery (CSRF)</a></li>
            </ul>
          </li>
          <li><a href="#references">Bug Bounty Programs</a></li>
        </ul>
      </td>
<!-- Image on the right -->
      <td>
        <p align="center">
          <img src="../assets/images/bug-guide.png" alt="bug-hunting" title="Bug Hunting" width="75%" />
        </p>
      </td>
    </tr>
  </table>
</div>

<script>
  // Get all elements with class 'dropdown-btn'
  var dropdownBtns = document.querySelectorAll('.dropdown-btn');
  
  dropdownBtns.forEach(function(btn) {
    btn.addEventListener('click', function() {
      // Toggle the dropdown content visibility
      var dropdownContent = this.nextElementSibling;
      if (dropdownContent.style.display === "none" || dropdownContent.style.display === "") {
        dropdownContent.style.display = "block";
      } else {
        dropdownContent.style.display = "none";
      }
    });
  });
</script>

---

## References

[OWASP Top 10](https://owasp.org/www-project-top-ten/)
<br>
[HackTricks](https://book.hacktricks.xyz/)
<br>
[Portswigger](https://portswigger.net/research)
<br>
[R-s0n GitHub](https://github.com/R-s0n)
<br>
[Back to top](#navigation)

---
