---
description: >-
  Recently a Reflected XSS vulnerability has been found in Flask-Admin v 1.5.2.
  This vulnerability is triggered by submitting a crafted URL which content is
  used in DOM.
---

# Reflected XSS

### Vulnerability



```python
# helpers.py
 
from urllib.parse import urlparse, urljoin
​
'....'
​
def get_redirect_target(param_name='url'):
    target = request.values.get(param_name)
​
    if target and is_safe_url(target):
        return target
​
def is_safe_url(target):
    ref_url = urlparse(request.host_url)
    test_url = urlparse(urljoin(request.host_url, target))
    return (test_url.scheme in ('http', 'https') and ref_url.netloc == test_url.netloc)
```

A function `get_redirect_target` takes the URL parameter named `url`, checks if it is safe, and returns the validated URL. This URL is further used in DOM elements to define the return\_url URL in navigation elements like `<a href=%return_url%`.

Developers were aware that the URL should have HTTP or HTTPS scheme and reside on the same network location with the current application, therefore, implemented the security checks.\
\
The URL check function `is_safe_url` checks if the domain is the same as the current domain and the scheme is HTTP or HTTPS.\
\
This check allows avoiding usage of different schemes like `file:` or `javascript:`. The `is_same_url` check relies on the URL parser to divide the `url` parameter into pieces, then compares the parts. The check expects the URL parsing library to behave uniformly in any case.

The current `is_safe_url` check can be bypassed by crafting the URL with a malicious `url` parameter.\
\
An incorrectly filtered `url` parameter is then inserted into the \<a href=%return\_url% tag using the template engine. If users open a malicious URL and then click on a tainted DOM element, they will trigger XSS.

\
