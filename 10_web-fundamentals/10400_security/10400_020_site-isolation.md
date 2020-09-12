# Site Isolation

On chrome 67 desktop, site isolation is enabled by defalut.

## What is site isolation

It ensures that pages from different websites are always put into different processes, each running in a sandbox that limits what the process is allowed to do. It also blocks the process from receiving certain types of sensitive data from other sites.

## Cross-Origin Read Blocking

### Why is it required

```markup
<img src="https://your-bank.example/balance.json">
<!-- Note: the attacker refused to add an `alt` attribute, for extra evil points. -->

<script src="https://your-bank.example/balance.json"></script>
```

With the above snippets, the contents of the JSON file would make it to the memory of the renderer process, at which point the renderer notices that it’s not a valid image format and doesn’t render an image. But, the attacker could then exploit a vulnerability like Spectre to potentially read that chunk of memory.

Instead of using `<img>`, the attacker could also use `<script>` to commit the sensitive data to memory

### What it is and what it does

Cross-Origin Read Blocking, or CORB, is a new security feature that prevents the contents of balance.json from ever entering the memory of the renderer process memory based on its MIME type.

A website can request two types of resources from a server:

* data resources such as HTML, XML, or JSON documents
* media resources such as images, JavaScript, CSS, or fonts

A website is able to receive data resources from its own origin or from other origins with permissive CORS headers such as `Access-Control-Allow-Origin: *`. On the other hand, media resources can be included from any origin, even without permissive CORS headers.

CORB prevents the renderer process from receiving a cross-origin data resource \(i.e. HTML, XML, or JSON\) if:

* the resource has an `X-Content-Type-Options: nosniff` header
* CORS doesn’t explicitly allow access to the resource

If the cross-origin data resource doesn’t have the X-Content-Type-Options: nosniff header set, CORB attempts to sniff the response body to determine whether it’s HTML, XML, or JSON. This is necessary because some web servers are misconfigured and serve images as text/html, for example.

Data resources that are blocked by the CORB policy are presented to the process as empty, although the request does still happen in the background. As a result, a malicious web page has a hard time pulling cross-site data into its process to steal.

For optimal security and to benefit from CORB, we recommend the following:

* Mark responses with the correct Content-Type header.
* Opt out of sniffing by using the `X-Content-Type-Options: nosniff` header. Without this header, Chrome does do a quick content analysis to try to confirm that the type is correct, but since this errs on the side of allowing responses through to avoid blocking things like JavaScript files, you’re better off affirmatively doing the right thing yourself.

## References

* Article on [https://developers.google.com/](https://developers.google.com/) titled as [Site Isolation for web developers](https://developers.google.com/web/updates/2018/07/site-isolation)

