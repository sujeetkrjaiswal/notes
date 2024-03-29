# Use of insufficiently random values

Insufficiently Random Values are an application security vulnerability whereby the application generates predictable values in sensitive areas of code that absolutely require strict randomness (unpredictability). As a result it may be possible for an attacker to predict the next value generated by the application to defeat cryptographic routines, access sensitive information, or impersonate another user.



### Code Sample

```javascript
const session = require('express-session')
const crypto = require('crrypto')
// ...
app.use(session({
  genid: function(req) {
    return easyToGuessGenerator() // BAD eg. counter+1
    // Dont use a pseudo random generator unless it is crypto safe.
    return crypto.randomBytes(256).toString('hex') // ACCEPTABLE
    // prefer using custom crypto-friendly pseudo random generators over default
  },
  secret: 'keyboard cat'
}))
```
