# Links Cross Origin

When you link to a page on another site using the `target="_blank"` attribute, you can expose your site to performance and security issues:

* The other page may run on the same process as your page. If the other page is running a lot of JavaScript, your page's performance may suffer.
* The other page can access your window object with the window.opener property. This may allow the other page to redirect your page to a malicious URL

## Preventing

* `rel="noopener"` prevents the new page from being able to access the window.opener property and ensures it runs in a separate process.
* `rel="noreferrer"` has the same effect but also prevents the Referer header from being sent to the new page

