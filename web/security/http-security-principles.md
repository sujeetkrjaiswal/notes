---
description: Basically use CSP headers
---

# HTTP Security Principles

### CSP Headers

TODO

### HTTP STRICT TRANSPORT SECURITY

**HTTP Strict Transport Security (HSTS)** is a mechanism that prevents user-agents (a browser or any kind of program designed for communication with a particular server) from browsing a website via an unencrypted connection in case an encrypted connection can be established, and only using a trusted certificate.

If the request is communicated through an unencrypted channel, it can be captured and tampered with by an attacker. The attacker then can steal or modify any information transmitted between the client and the server or redirect the user to a phishing website. So, the first goal of HSTS is to ensure traffic is encrypted, so it instructs the browser to always use HTTPS instead of HTTP.

Usually, browsers allow users to ignore TLS errors and continue browsing potentially insecure websites. With HSTS enabled, the user will be unable to skip the browser warning and continue. The second important goal of HSTS is to make sure that the traffic is encrypted using a trusted and valid certificate.

When the HSTS response header signals the browser that the certain domain must be requested only using HTTPS, the browser saves this domain to the HSTS list and keeps it there for the timeframe specified in `max-age` directive of the Strict-Transport-Security header.\
\
There are two cases when HSTS doesn't provide proper protection:

* when the user hasn't browsed the website before and is making his very first request to this website over HTTP,
* when existing HSTS data has already expired.

To resolve this, it is possible to have a domain included in the HSTS preload list which arrives baked into most modern browsers.

### X-XSS-Protection

Some modern browsers have built-in XSS protection mechanisms that can be used as an additional layer of security against Reflected XSS. The main problem with that is that all of the browsers implement built-in XSS filtering differently, so to add more control to the process and make sure that the loading of a page with the malicious content will be blocked, the X-XSS-Protection header is needed.

X-XSS-Protection header is an optional HTTP header that performs XSS filtering by defining the anti-XSS mechanism behavior: from sanitizing the page by blocking injected Javascript to preventing page rendering and reporting the violation.

By default, browsers that support XSS filtering have it enabled. Though it can be disabled, this is considered a bad practice; often, if an application requires XSS protection to be disabled in order to function properly, it is an indication that the application is quite likely vulnerable to XSS.

Please note that only using the X-XSS-Protection header will not protect your application from XSS, but this header will make an important input in your defense-in-depth strategy and make it more robust.

### X-FRAME-OPTIONS

X-Frame-Options header defines if the webpage can be rendered inside an `<iframe>`, `<frame>`, `<applet>`, `<embed>` or `<object>` tags. Depending on the directive, this header either specifies the list of domains that can embed the webpage, or allows the page to be embedded only inside pages of the same origin, or totally prohibits embedding of a webpage.\
\
The main purpose of the X-Frame-Options header is to protect against Clickjacking. Clickjacking is an attack when the vulnerable page is loaded in a frame inside the malicious page, and the users are tricked into interaction with buttons and other clickable UI elements (e.g. unknowingly clicking "likes" or downloading malicious files) of a vulnerable page without their knowledge.\
\
(Note that setting X-Frame-Options with `<meta>` HTML tag inside a webpage doesn't work. Use HTTP headers to set it.)

#### X-FRAME-OPTIONS AND CONTENT-SECURITY-POLICY

X-Frame-Options is also covered by Content-Security-Policy (CSP). CSP is a suite of headers with security directives for multiple uses (which will be covered in their own lesson). Among these security directives is the `frame-ancestors`directive which pursues the same goal as the X-Frame-Options header. The main difference between them is the implementation of the `SAMEORIGIN` directive of the X-Frame-Options header.\
\
Various user-agents interpret `SAMEORIGIN` directive in a different way, and some of them only check the top-level domain. Whereas with the CSP's `frame-ancestors` directive, the whole chain of origins is checked. (The example of the chain of origins is an iframe inside an iframe, inside an iframe, and so on.)\
\
The CSP W3C recommendation states that with the CSP `frame-ancestors` directive being introduced, the X-Frame-Options header becomes obsolete. Thus CSP should be used to prevent Clickjacking in the first place and the X-Frame-Options header should be used for backward compatibility with browsers that don't support this CSP directive.

### References

* [MDN Docs for CSP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
