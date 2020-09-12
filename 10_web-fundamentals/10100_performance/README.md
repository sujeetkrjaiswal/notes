# Performance

## Categorization of Performance impovements & bottlenecks

* Network
  * Use CDN
* App Development
  * Tree Shaking
  * Lazy loading of resources \(Js, Css, etc\)
  * Bundling multiple small files into one
  * Minification of resorces with comment removal and .map files for stage only
  * Files names to use content-hash to enable longer caching
* App Serving
  * Compression: `Gzip` & `brotly`
  * Server side rendering for perceived performance imporvement and SEO
  * TODO
* Caching
  * Appropritate Headers
  * Cache invalidation
  * Service worker caching
  * Using index db to cache data for common use items
* Image Optimization
  * Lazy loading of images \(`loading=lazy`\)
  * using img srcset to load images of required size only
  * Image compression - lossy & lossless
  * Use Sprites for very small images & icons
  * Use Image interlacing
  * Next gen formats of images \(JPEG 2000, JPEG XR, and WebP are image formats that have superior compression and quality characteristics compared to their older JPEG and PNG counterparts. Encoding your images in these formats rather than JPEG or PNG means that they will load faster and consume less cellular data. WebP is supported in Chrome and Opera and provides better lossy and lossless compression for images on the web\)
* Resoure Hints
  * Preload resources \(CSS/Fonts/JS\) \(updated version of subresource prefetching - depricated in 2016\)
  * 
* Memory leakages for large app
  * When using stores, invalidating or removing states that are not usefult
  * Localising states unless truly required
  * Adding/removing listeners
  * Use Chrome Dev tools \(Memory tab\) to observe Heap size
* Coding imporvement
  * Transfer computations/bulk operations to server if not truely required to be done on browser
  * Use web workers for running synchronous process
  * Use baching of long-running synchronous process, if web workers can not be used
  * Using pagination over infinite scroll unless the functionality really needs it. \(More DOM will create issues\)
  * For cases where large number of dom elements rendered, use windowing libraries like react-window or react-virtualized
  * Do not use passive listeners - to improve scrolling performance
  * Using lazy-synced inputs \(onBlur/debounced\) & local states for forms
  * XHR/Fetch cancellation on page change \(use AbortController signal with fetch\)
  * Parameterized APIs/API wrappers to filter out unnecessary data from server or via post-processing in the API response function

## Tools to use to evaluate performance/best practicses

* [https://web.dev/measure/](https://web.dev/measure/)
* Chrome Dev tools \(Audit tab\) / Lighthouse

## Refrences

* [https://web.dev](https://web.dev)

