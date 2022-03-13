# Direct (Path) Traversal



### Bad code

```javascript
var fs = require('fs')
​// sample url = https://some.domain/some-path?file=2234
function handleFileRequest(req, res, next) {
    var filename = req.query.file,
        filePath = `/tmp/${filename}`;
​
    fs.readFile(filePath, 'utf8', function(err, data) {
        if (err) {
            next(err);
        } else {
            res.end(data);
        }
    });
}
​
```

To get the list of password, you can try below

```
<URL_PATH>?file=../etc/passwd
```

### Fixed Code

As with any user-supplied input, it is important to ensure that there is a context-specific input validation strategy in place.

In the case, an obvious solution would be to, first, canonicalize the full path name and then - to validate that the canonicalized path is in an intended/allowed directory on the file system.

```javascript
var fs = require('fs')
    path = require('path');
​
function handleFileRequest(req, res, next) {
    var filename = req.query.file,
        filePath = `/tmp/${filename}`;
        normalizedPath = path.normalize(filePath);
​
    if (normalizedPath.indexOf('/tmp/') !== 0) {
        next(new Error("Unauthorized Access"));
        return;
    }
​
    fs.readFile(filePath, 'utf8', function(err, data) {
        if (err) {
            next(err);
        } else {
            res.end(data);
        }
    });
}
```

