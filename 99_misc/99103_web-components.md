# Web Components

description: About web component

## Guidelines for web components

* Always scope your styles, because if not scoped and used global css, what happens when it is used inside another wc which is shadowed
* Use shadow Dom with WC which have children, implementation should always be hidden
* For primitive data types, use it as both Attributes and Properties, except when it has high frequency change
  * Dont do for properties that has high change frequency
  * Dont do that for object/array, since serialized object will loose its identity
* Accept primitive data as either attributes or properties.
* Reflect primitive data from attribute to property, and vice versa
* Only accept rich data as properties
* Do not reflect rich data from properties to attribute
* Do not dispatch event sin response to the host setting a property \(downward data flow\)
* Do dispatch events in response to internal element activity\(Basically when the element knows something changed, but the host does not\)

Sample implementations:

```javascript
class CustomVideo extends HTMLElement {
  get preload() {
    const value = this.getAttribute('preload');
    return value === null ? 'auto' : value;
  }
  set preload() {
      this.setAttribute('preload', value);
  }
}
```

