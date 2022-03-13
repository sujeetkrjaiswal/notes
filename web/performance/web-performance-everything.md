---
description: >-
  Everything you need to know to measure and improve the performance of any web
  application.
---

# Web Performance: Everything you need to know

Web performance is important since user conversion & retention is directly proportional to performance and many search engine including google ranks your site based on performance metrics. We will focus on all the factors that you can address to improve the performance of your site in the first half of the article and in the second half we will go through all the metrics that are important for web performance.

## Improving Performance

### Areas of improvement

* Optimizing Images
  * Lazy loading of images (`loading=lazy` attribute) or use Intersection Observer to load when the image enters the viewport
  * using `img-srcset` to load images of required size only (requires to create multiple images of different sizes)
  * Image compression - lossy & lossless
  * Use Sprites for very small images & icons. Prefer fonts for icons if possible
  * Use Image interlacing
  * Next-gen formats of images (JPEG 2000, JPEG XR, and WebP are image formats that have superior compression and quality characteristics compared to their older JPEG and PNG counterparts. Encoding your images in these formats rather than JPEG or PNG means that they will load faster and consume less cellular data. WebP is supported in Chrome and Opera and provides better lossy and lossless compression for images on the web)
  * pre-fetch images required for the next pages.
* Reducing Download Size
  * Compression - Use `Gzip` and `brotli` compression to reduce the content size to download.
  * Lazy load resources - image, components, etc.
  * Caching
  * For external resources like fonts, use the one from a CDN, it might be already downloaded. &#x20;
* Progressive Rendering
  * Use Above the fold design guidelines to enable rendering of critical content
  * Use inline or Internal style for Above-the-fold design to mitigate Flash of unstyled content (FOUC)
  * App Shell model for rendering the Single Page Application (SPA) and Progressive Web Applications (PWA)
* Networking
  * Use CDN (Content Delivery Network): Reduces latency.
  * DNS-prefetch
  * Reduce no. of concurrent requests made by the applications. Only a specific no. of concurrent calls are made and others are placed in a queue.
* Server Sider Improvements
  * Server-Side Rendering (e.g. Next.js)
  * Static Site Generation (e.g. Gatsby for react)
  * Serve your static content from memory instead of reading from the file-system on each request.
  * Use Graphql to merge multiple requests into one OR create wrapper APIs which internally makes multiple calls to different services and provide the data in one go to the front-end applications.
* Cache everything you can
  * Use CDN for caching all the static content.
  * Cache on the browser using cache headers (cache-control)
  * Using service workers to cache content programmatically.
  * N + 1 caching strategy: Everything will be severed via cache if available and in the background make the actual network call to update the content so that next time, the user is guaranteed to view the updated content.
  * Use content hash in the file name (eg. file-name.\[content-hash].extension) to enable caching of static files for infinite time.
* Resource Hints
  * pre-load\
    `<link rel="preload" href="https://url/to/font.woff" as="font" crossorigin>`
  * pre-fetch
    *   link pre-fetching\
        `HTML: <link rel="prefetch" href="/url/to/fetch.extenstion">`

        `HTTP header: Link: </url/to/fetch.extenstion>; rel=prefetch`
    * dns-prefetch `<link rel="dns-prefetch" href="//domain.name">`
  * pre-connect
  * pre-render\
    `<link rel="prerender" href="https://url.render/to/page">`
* Take care of Blocking resources
  * Use `async` and `defer` with `script` tag to make them non-blocking
  * Put only the most important scripts and stylesheets in the head, everything else can be moved as the last child of `body` tag.
  * Use `differential loading` strategy to load or not load polyfills. You can explore `module` script-type and `nomodule`  attribute to detect if the user is running a modern browser.
* Build time improvement
  * Tree Shaking
  * Lazy loading of resources (Js, CSS, etc.)
  * Bundling multiple small files into one
  * Minification of resources with comment removal and .map files for stage only
  * Files names to use content-hash to enable longer caching
* Improvement in code/programming
  * Transfer computations/bulk operations to the server if not truly required to be done on the browser
  * Use web workers for running synchronous process
  * Use baching of long-running synchronous process, if web workers can not be used
  * Using pagination over infinite scroll unless the functionality really needs it. (More DOM will create issues)
  * For cases where a large number of dom elements rendered, use windowing libraries like react-window or react-virtualized
  * Do not use passive listeners - to improve scrolling performance
  * Using lazy-synced inputs (onBlur/debounced) & local states for forms
  * XHR/Fetch cancellation on page change (use AbortController signal with fetch)
  * Parameterized APIs/API wrappers to filter out unnecessary data from the server or via post-processing in the API response function

### Things to avoid or look out for

* Memory leakages for large app
  * When using stores, invalidating or removing states that are not required
  * Localizing states unless truly required to be global
  * Removing listeners, when not required.
  * Use Chrome Dev tools (Memory tab) to observe Heap size
* When animating using JS
  * Prefer using CSS Animations unless js is the only option to go for
  * Using `requestAnimationFrame` instead of `setInterval`
  * Making sure the computation done is minimal inside the animation logic in order to make sure frame rates are not dropped.
* Overwriting default behavior (eg. scrolling)
  * use debounce and/or throttling&#x20;
  * reduce computation inside the event listener callback function

### What tools can you use to improve the performance

* Webpack or similar tools to bundle, tree-shake, and lazy load applications
* Image pre-processing tools like `GraphicsMagick` to generate multiple images, interlacing, and compression

## Measuring Performance

### Types of metrics

* **Perceived load speed**: How quickly page can load and render visual elements
* **Load responsiveness**: How quickly the page can load and execute javascript to respond to user interactions.
* **Runtime responsiveness**: After page load, how fast the page can respond to user interaction
* **Visual stability**: Unexpected page sifts and potentially can interfere with user interactions
* **Smoothness**: Consistent frame rate for animations and transition. Fluid flow between two states.&#x20;

### Performance Metrics

* **Time To First Byte (TTFB)**: Time to receive the first byte of the page.
* **First Contentful Paint (FCP)**: Time for page's first element to render.
* **First Meaningful Paint (FMP)**: Time for page's primary content to render. FMP is now deprecated in favor of LCP.
* &#x20;**Largest Contentful Paint (LCP)** measures loading performance (< 2.5sec ). Time to load the largest text block or image to load.
* **First Input Delay (FID)** measures interactivity (< 100 ms). Time difference between first user interaction and when the browser actually responded to the interaction.
* **Time to Interactive (TTI)**: Time to reliably responding to interactions (after the entire site is rendered and all initial scripts loading and execution is done).
* **Total Blocking Time (TBT)**: Total time between FCP and TTI.
* **Cumulative Layout Shift (CLS)** measures visual stability (< 0.1). A Cumulative score of all unexpected page shifts that occur between page load and when its lifecycle state changes to hidden.

### Measuring performance tools

* [https://web.dev/measure/](https://web.dev/measure/) An online tool to measure Performance, Accessibility, Best Practices, and SEO.
* Chrome Dev tools (Audit tab) / Lighthouse
* Performance Tab in Chrome Dev Tools

## References

* [https://web.dev](https://web.dev)
* [https://developers.google.com/web](https://developers.google.com/web)
