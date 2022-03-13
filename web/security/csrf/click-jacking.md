# Click Jacking

```javascript
var helmet = require('helmet')
app.use(helmet({
    frameguard: { 
        'action': 'deny'
     },
    contentSecurityPolicy: { 
        'frame-ancestors': 'none' 
    }
})) 
```

Value for `frame-ancestors`:

* `none` is the same as DENY
* `self` is the same as SAMEORIGIN
* Source URI - enables the server administrator to specify scheme, host, and port of allowed parent pages. This directive permits usage of wildcards, e.g. https://\*.codebashing.com.

