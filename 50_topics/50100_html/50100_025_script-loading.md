# Script Loading

## Browser differences in module and regular script

```markup
<script type="module">
    console.log(`From 1st script: ${typeof button}`)
</script>
<script>
    console.log(`From 2nd script: ${typeof button}`)
</script>
<button id="button">Button</Button>
```

Output:

```text
From 2nd script: undefined
From 1st script: Object
```

### Hints

* `module` scripts are always `deferred`
* They always `use strict`
* `this` inside module is always undefined. Why?? Because of `use strict`
* relative order of scripts is maintained \(For type=module only & if deferred\), For above case, 2nd script tag will execute first, since its deferred.

## Async / Deffered

* `async` attribute only works on external scripts, and run immediately when ready, independently or other scripts or the document. But for `type=module`, it works for inline scripts as well and it waits for all the dependencies are fetched and then it runs, but doesn't wait for document or any other `<script>` tags.

### External Scripts

```markup
<script type="module" src="my.js"></script>
<script type="module" src="my.js"></script>
```

* the script my.js is fetched and executed only once.

```markup
<script type="module" src="http://another-site.com/their.js"></script>
```

* `another-site.com` must supply `Access-Control-Allow-Origin` otherwise, the script won't execute.

### No bare modules

```javascript
import {sayHi} from 'sayHi' // Error, "bare" module
```

The module must have a path, e.g `'./sayHi.js'`

### Module/Nomodule: Differential loading

```markup
<script type="module">
  console.log("Runs in modern browsers");
</script>

<script nomodule>
  console.log("Modern browsers know both type=module and nomodule, so skip this")
  console.log("Old browsers ignore script with unknown type=module, but execute this.");
</script>
```

