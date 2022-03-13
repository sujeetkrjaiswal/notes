# Insecure URL Redirects

### Code

#### App with issue

```markup
<a class="button greyishB" href="/redirect?url=https://sso.codebashing.com">
    <span>Backoffice SSO Login</span>
</a>
```

Node js redirect code

```javascript
app.get('/redirect', function (req, res, next) {
    var url = req.query.url;
    if (url) {
        res.redirect();
    } else {
        next();
    }
});

```

#### Malcious site

The bad folks created a clone site and hosted it and then sent a link to a user

```
https://tradeworx.codebashing.com/traderworx/redirect?url=https://evil.codebashing.com
```

#### Fix

use a token or whitelist allowed urls
