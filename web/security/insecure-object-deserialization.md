# Insecure Object Deserialization



Most programming languages including NodeJS provide ways for users to serialize objects into the text or binary format that can be saved or transmitted through the network and then construct those objects back using the deserialization process. This is a very common coding practice.\
\
Vulnerabilities arise when developers try to construct an object from an untrusted serialization stream, and they assume that this stream is always correct.\
\
If the stream was corrupted, tampered with or replaced prior to deserialization, the deserialized objects may have an unexpected or illegal state.\
\
The nastiest consequence of insecure object deserialization is Remote Code Execution.\
\
If you can make some server execute the remote command - then you have a foot in the doorway, and can do anything to this server:

* Upload and execute a backdoor.
* Use the server as a cryptocurrency miner.
* Use the server as a part of a botnet.

```javascript
function createAccountManager(req, res) {
  var body = req.body.NewAccountManager //requires body-parser package
  var am = serialize.unserialize(body)
  var status = addAccountManager(am)
  if (status) {
    // ... Handle valid object ... //
  }
  else {
    // ... Handle invalid object ... //
  }
}
​
```

#### FIXED

```javascript
function createAccountManager(req, res) {
  var body = req.body.NewAccountManager //requires body-parser package
  // var am = serialize.unserialize(body)
  var am = JSON.parse(body)
  var status = addAccountManager(am)
  if (status) {
    // ... Handle valid object ... //
  }
  else {
    // ... Handle invalid object ... //
  }
}
​
​
```
