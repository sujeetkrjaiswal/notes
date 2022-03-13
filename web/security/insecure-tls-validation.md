# Insecure TLS Validation



```javascript
var https = require('https');
var url = require('url');
​
var options = {
  host: 'trade-ssl.com',
  port: 443,
  method: 'GET',
};
​
var req = https.request(options, function(res) {
     
  // Do something with the response
​
});
req.end();
```

In this example, some action is performed over HTTPS, but no validation of a peer certificate is performed. Even the expiration date of the certificate is not checked.

As a result, by leveraging this badly designed code an attacker can now use an expired certificate, revoked certificate, self-signed certificate or no certificate at all and still pass through the TLS validation without being blocked.

#### Valid code

```javascript
var https = require('https');
var url = require('url');
var options = {
  
  host: 'trade-ssl.com',
  port: 443,
  method: 'GET',
  rejectUnauthorized:true // To avoid self-signed certs
};
var req = https.request(options, function(res) {
  var cert = res.connection.getPeerCertificate();
  var SANs = cert.subjectaltname;
  var naked_domain = 'trade-ssl.com';
  var wildcard_domain = '*.trade-ssl.com';
•
  if(cert.valid_from > current_date || cert.valid_to < current_date)
  {
    return false;
  }
  if (SANs.indexOf(naked_domain) < 0 && SANs.indexOf(wildcard_domain) < 0)
  {
    return false;
  }
  return true
  // Do something with the response
});
req.end();
​
```
