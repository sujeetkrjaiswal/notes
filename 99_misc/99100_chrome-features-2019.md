# Chrome Features 2019

## 1. Numeric separators

```javascript
const num1 = 10_000_000n;
```

## 2. Intl stuff

```javascript
const rtf = new Intl.RelativeTimeFormat('en')
rtf.format(3.14, 'seconds') // -> in 3.14 seconds
rtf.format(-15, 'minute') // 15 minutes ago

const srtf = new Intl.RelativeTimeFormat('es')
srtf.format(-2, 'year') // hace 2 anos

const lf = new Intl.ListFormat('en')
lf.format(['Paul', 'Jake']) // Paul and Jake
lf.format(['Paul', 'Jake', 'Surma', 'Mariko']) // Paul, Jake, Surma and Mariko

const lf_en = new Intl.ListFormat('en', {type: 'disjunction'})
lf_en.format(['Surma', 'Mariko']) // Surma or Mariko

new Intl.NumberFormat('en', {style: 'unit', unit: 'second'}).format(2000) // "2,000 sec"
new Intl.NumberFormat('de', {style: 'unit', unit: 'second'}).format(2000) // "2.000 Sek."
```

## 3. Match All

```javascript
const str = 'Hello world. Hello everyone.'
str.match(/Hello\s(\S+)/g) // ['Hello world', 'Hello everyone']
for(const result of str.matchAll(/Hello\s(\S+)/g)) {
    // {0: 'Hello world', 1: 'world', index: 0}
    // {0: 'Hello everyone', 1: 'everyone', index: 13}
    // PS: it returns an RegExStringIterator and result is an Array with index 0,1 etc and properties like index, input, and groups
}
```

## 4. Class fields

```javascript
class SuperClass {
    foo = (() => console.log('foo initialized on SuperClass'))();
    constructor() {console.log('Superclass constructed')}
}
class Whatever extends SuperClass {
    foo = (() => console.log('foo initialized on whatever'))()
    constructor() {
        console.log('Whatever constructed')
        super()
        console.log('super called');
    }
}
new Whatever()

// Logs:
/**
Whatever constructed
foo initialized on SuperClass
Superclass constructed
foo initialized on whatever
super called
**/
```

Explanation: For a class when the constructor is called it executes till the super\(\), after that initalizes the fields and after that resumes execution of the constructor.

### private class fields

```javascript
class Whatever {
    #counter = 0;
    increment() {
        this.#counter++
    }
    get counter() {
        return this.#counter
    }
}

const whatever = new Whatever()
console.log(whatever.#counter) // Syntax Error
whatever.#counter = -1 // Syntax Error
console.log(whatever.counter) // 0
```

## 5. Image Aspect Ratio

```markup
<div>
    <img src="cats.jpg" width="800" height="600">
    <p>Page content</p>
</div>
<style>
    div {
        width: 400px;
    }
    img {
        width: 100%;
        height: auto;
    }
</style>
```

Setting `height` as `'auto'` explicitly uses the img attribute `height` and `width` to compute the aspect ratio and then uses that value in the css to compute the `height`

## 6. Blob Reading

```javascript
const blob = input.files[0]
const result = await new Response(blob).text() // Earlier way of doing stuff or use FileReader Api
const result = await blob.text(); // or stream() or arrayBuffer()
```

## 7. Images in clipboard

```javascript
try {
    const imgURL = '/images/generic/file.png'
    const response = await fetch(imgURL)
    const blob = await response.blob()
    await navigator.clipboard.write([ // Requries user permission
        new ClipboardItem({[blob.type]: blob})
    ])
    console.log('Image copied')
} catch (error) {
    console.error('Boo!', error)
}
```

### Getting all the items from clipboard

```javascript
try {
    const items = await navigator.clipboard.read()
    for(const item of items) {
        for(const type of item.types) {
            const blob = await item.getType(type)
            console.log(type, blob)
        }
    }
} catch (err) {
    console.error('Boo!', error)
}
```

## 8. Sharing

When you want to share something to other websites/apps

```javascript
try {
    await navigator.share({
        files: filesArray,
        title: 'Holiday photos'
    })
    console.log('success')
} catch (error) {
    console.log('Boo!', error)
}
```

When you want to receive sharable data from other websites \(in PWAs\). Use the below config in your manifest.json

```javascript
{
    "share_target": {
        "action": "/?share-target",
        "method": "POST",
        "enctype": "multipart/form-data",
        "params": {
            "files": [
                {
                    "name": "file",
                    "accept": ["image/*"]
                }
            ]
        }
    }
}
```

Using above stuff will be HTTP POSTed to your app. winch you can intercept with a service worker.

## 9. Backdrop filter

```css
.element {
    backdrop-filter: blur(5px);
}
```

## 10. All Settled

```javascript
const p = await Promise.all([
    Promise.resolve("a"),
    Promise.reject("B")
])
// p rejects with b because of short-circit

const p2 = await Promise.allSettled([
    Promise.resolve("a"),
    Promise.reject("b")
])
// p resolves with
/**
[
    {"status": "fulfilled", "value": "a"},
    {"status": "rejected", "reason": "b"}
]
*/
```

## 11. Media keys \(in the keyboard\)

When you are playing an audio or video, you have a media session. When you have a media session, you can opt in to listen to media keys in the keyboard.

```javascript
navigator.mediaSession.setActionHandler('pause', () => {
    // do something
})
```

## 12. Background fetch

```javascript
// From a page or worker
navigator.serviceWorker.ready.then(async swReg => {
    await swReg.backgroundFetch.fetch(
        'my-fetch', ['/ep-5.mp3', 'ep-5-artwork.jpg'],
        {
            title: 'Episode 5: Interesting things',
            icons: [{
                sizes: '300x300',
                src: '/ep-5-icon.png',
                type: 'image/png'
            }],
            downloadTotat: 60 * 1024 * 1024,
        }
    )
})

// Im the service worker
addEventListener('backgroundfetchsuccess', event => {
    const bgFetch = event.registration
    event.waitUnitl(async function(){
        const records = await bgFetch.matchAll()
        event.updateUI({
            title: 'Episode 5 ready to listen'
        })
    }())
})
addEventListener('backgroundfetchclick', event => {
    const bgFetch = event.registration
    if(bgFetch.result === 'success') {
        clients.openWindow('/latest-podcasts')
    } else {
        clients.openWindow('/download-progress')
    }
})
```

## 13. Prefers styles

```css
@media (prefers-color-scheme: dark) {
    /** ...  */
}
@media (prefers-color-scheme: light) {
    /** ... */ 
}
@media (prefers-reduced-motion: reduce) {
    /** ... */
}
```

```javascript
if(window.matchMedia('(prefers-color-scheme: dark)').matches) {
    // make the adjustments
}
```

## 14. Native Lazy Loading

Browsers will not load images untill you scroll them into view. It works on iframe too.

```markup
 <img src="image.webp" loading="lazy">
```

## 15. Form elements

When using a form, it only considers the input values for form submission and ignores any custom element. You can now use the values from such elements as well

```javascript
form.addEventListener('formdata', event => {
    event.formData.append('my-input', myInputValue)
})
```

Example in using custom element

```javascript
class MyCheckbox extends HTMLElement {
    static formAssociated = true

    constructor() {
        super()
        this._internals = this.attachInternals()
    }
    get form() {return this._internals.form}
    whatever() {
        this._internals.setFormValue('yes please')
        this._intervals.setValidity(...);
    }
}
```

## 16. fromEntries

```javascript
    const obj = {a:1, b:2, c:3}
    const list = Object.entries(obj) // [['a',1], ['b',2], ['c',3]]
    const obj2 = Object.fromEntries(list) // {a:1, b:2, c:3}
```

