# DOM XSS

To defend against Cross-Site Scripting attacks in a Document Object Model (DOM) environment, a defense-in-depth approach is required, combining several security best practices.\
\
You should recall that Stored XSS and Reflected XSS injections take place server-side rather than client-side. With DOM XSS, the attack is injected into the browser’s DOM thus adding complexity by making it very difficult to prevent and highly context specific (because an attacker can inject HTML, HTML Attributes, or CSS as well as URLs).\
\
As a general set of principles, the application should first HTML-encode and then JavaScript-encode any user-supplied data that is returned to the client.\
\
Due to the very broad attack surface, developers are strongly encouraged to review areas of code that are potentially susceptible to DOM XSS, including but not limited to:\
\
`window.name` `document.referrer` `document.URL` `document.documentURI` `location` `location.href` `location.search` `location.hash` `eval` `setTimeout` `setInterval` `document.write` `document.writeIn` `innerHTML` `outerHTML`\
