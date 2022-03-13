---
description: XML External Entity
---

# XXE Processing

### What

XML External Entity (XXE) Processing is a type of application security vulnerability whereby a malicious user can attack poorly configured/implemented XML parser within an application. Malicious external entity references can be forced by an attacker, which results in unauthorized read-access to sensitive files on the server that the XML parser runs from. Denial of Service is another potential outcome.&#x20;

### How

By using XML entities, the `SYSTEM` keyword causes an XML parser to read data from a `URI` and permits it to be substituted in the document.\
\
Simply put the XML parser was tricked into accessing a resource that the application developer's did not intend to be accessible, in this case a file on the local file system of the remote server.\
\
In this example, the following XML code fetched the `/etc/passwd` file present on BatchTRADER's web server and displayed it to the user of the application.\


```
<!ENTITY bar SYSTEM "/etc/passwd" >]>
```

Because of this vulnerability any file on the remote server (or more precisely, any file that the web server has read access to) could be obtained.\
\
A malicious attacker could easily use this to gain access to arbitrary files such as system files, or files containing sensitive data such as passwords or user data.\


```markup
<!DOCTYPE foo [<!ELEMENT foo ANY >
<!ENTITY bar SYSTEM "file:///etc/passwd" >]>
<trades>
    <metadata>
        <name>Apple Inc</name>
        <stock>APPL</stock>
        <trader>
            <foo>&bar;</foo>
            <name>C.K Frode</name>
        </trader>
        <units>1500</units>
        <price>106</price>
        <name>Microsoft Corp</name>
        <stock>MSFT</stock>
        <trader>
            <name>C.K Frode</name>
        </trader>
        <units>5000</units>
        <price>45</price>
        <name>Amazon Inc</name>
        <stock>AMZN</stock>
        <trader>
            <name>C.K Frode</name>
        </trader>
        <units>4500</units>
        <price>195</price>
    </metadata>
</trades>





```

In NodeJS, the most robust method to protect against XXE attacks is to configure the applications XML parser to not allow `DOCTYPE` declarations.\
\
This is done by invoking the `noent` configuration parameter to `false`\
\
With this set, an exception occurs if our `trade.xml` contains a `DOCTYPE` declaration and parsing stops, preventing the vulnerability from exposing sensitive information.

```javascript
var parserOptions = {
    noblanks: true,
    noent: false, // earlier it was true
    nocdata: true
};
​
try {
    var doc = libxml.parseXmlString(data, parserOptions);
} catch (e) {
    return Promise.reject('Xml parsing error');
}
​
​
```

TODO
