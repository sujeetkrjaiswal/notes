# To Do

## Font: Critical hidden sub-resource

Browser doesnt dispatches a request for font unless the render tree is made since multiple fonts are specified in css but only one/two are being used.

```markup
<link rel="preload" href="./some-style.css" as="style">
<link rel="preload" href="./some-font.woff" as="font" crossorigin>
<link rel="preload" href="./some-data.json" as="fetch">
<link rel="preload" href="./some-module.mjs" as="modulepreload">
```

## Things to explore

* `navigator.sendBeacon('/end-of-session')`
* [use the pagehide event over beforeunload or unload](https://www.igvita.com/2015/11/20/dont-lose-user-and-app-state-use-page-visibility/)
* [pageshow](https://developer.mozilla.org/en-US/docs/Web/Events/pageshow), [pagehide](https://developer.mozilla.org/en-US/docs/Web/Events/pagehide), [beforeunload](https://developer.mozilla.org/en-US/docs/Web/Events/beforeunload), [unload](https://developer.mozilla.org/en-US/docs/Web/Events/unload)

