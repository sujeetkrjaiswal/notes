# Resource Hints

Resource hints, for example, `preconnect` and `prefetch`, are executed as the browser sees fit. The `preload`, on the other hand, is mandatory for the browser. Modern browsers are already pretty good at prioritizing resources, that's why it's important to use `preload` sparingly and only `preload` the most critical resources.

Unused preloads trigger a Console warning in Chrome, approximately 3 seconds after the load event.

## 1. Preload

* Some types of resources, such as fonts, are loaded in `anonymous` mode. For those you must set the crossorigin attribute with preload.
* `<link>` elements also accept a `type` attribute, which contains the MIME type of the linked resource. The browsers use the value of the `type` attribute to make sure that resources get preloaded only if their file `type` is supported. If a browser doesn't support the specified resource `type`, it will ignore the `<link rel="preload">`

Using headers instead of `link` has one benifit that the browser doesn't need to parse the document to discover it, which can offer small improvements in some cases.

```yaml
Link: </css/style.css>; rel="preload"; as="style"
```

### Preloading resources defined in CSS

Fonts defined with @font-face rules or background images defined in CSS files aren't discovered until the browser downloads and parses those CSS files. Preloading these resources ensures they are fetched before the CSS files have downloaded.

Browser doesnt dispatches a request for font unless the render tree is made since multiple fonts are specified in css but only one/two are being used.

**Fonts preloaded without the crossorigin attribute will be fetched twice!**

```markup
<link rel="preload" href="./some-font.woff" as="font" crossorigin>
<link rel="preload" href="./some-font.woff2" as="font" type="font/woff2" crossorigin>
```

### Preloading CSS files

If you are using the critical CSS approach, you split your CSS into two parts. The critical CSS required for rendering the above-the-fold content is inlined in the `<head>` of the document and non-critical CSS is usually lazy-loaded with JavaScript. Waiting for JavaScript to execute before loading non-critical CSS can cause delays in rendering when users scroll, so it's a good idea to use `<link rel="preload">` to initiate the download sooner.

```markup
<style type="text/css">
.class-imp {
  /* Classes important for first page render */
}
</style>

<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="styles.css"></noscript>
```

The above snippet is explained in the [render blocking article](https://github.com/sujeetkrjaiswal/notes/tree/8679ddf73b6dc6ea01c5629600589ca0623e216f/10_web-fundamentals/10100_performance/render-blocking.md#Defer%20non-critical%20Stylescheets)

### Preloading JavaScript files

```markup
<link rel="preload" as="script" href="critical.js">
<link rel="preload" href="./some-data.json" as="fetch">
<link rel="preload" href="./some-module.mjs" as="modulepreload">
```

For webpack use the below magic comments for newer versions and for older use the plugin `preload-webpack-plugin`.

```javascript
import(_/* webpackPreload: true */_ "CriticalChunk")
```

## 2. Prefetch

`prefetch` is a low priority resource hint that allows the browser to fetch resources in the background \(idle time\) that might be needed later, and store them in the browser's cache. Once a page has finished loading it begins downloading additional resources and if a user then clicks on a prefetched link, it will load the content instantly.

### 2.1 Link Prefetching

link prefetching allows the browser to fetch resources, store them in cache, assuming that the user will request them. The browser looks for prefetch in the `<link>` HTML element or the Link `HTTP header` such as:

```markup
<link rel="prefetch" href="/uploads/images/pic.png">
```

```yaml
Link: </uploads/images/pic.png>; rel=prefetch
```

### 2.2 DNS Prefetching

DNS prefetching allows the browser to perform DNS lookups on a page in the background while the user is browsing. This minimizes latency as the DNS lookup has already taken place once the user clicks on a link. DNS prefetching can be added to a specific URL by adding the rel="dns-prefetch" tag to the link attribute. We suggest using this on things such as Google fonts, Google Analytics, and your CDN.

```markup
<!-- Prefetch DNS for external assets -->
<link rel="dns-prefetch" href="//fonts.googleapis.com">
<link rel="dns-prefetch" href="//www.google-analytics.com">
<link rel="dns-prefetch" href="//cdn.domain.com">
```

## 3. Preconnect

The preconnect directive allows the browser to setup early connections before an HTTP request is actually sent to the server. This includes DNS lookups, TLS negotiations, TCP handshakes. This in turn eliminates roundtrip latency and saves time for users.

```markup
<link href="https://cdn.domain.com" rel="preconnect" crossorigin>
```

## 4. Prerendering

Prerendering is very similar to prefetching in that it gathers resources that the user may navigate to next. The difference is that prerendering actually renders the entire page in the background, all the assets of a document.

```markup
<link rel="prerender" href="https://www.keycdn.com">
```

## 4. FAQs

### Preload vs Prefetch

`preload` is different from `prefetch` in that it focuses on fetching a resource for the current navigation. `prefetch` focuses on fetching a resource for the next navigation. It is also important to note that `preload` does not block the window's onload event.

## 5. References

* [https://www.keycdn.com/blog/resource-hints](https://www.keycdn.com/blog/resource-hints)

