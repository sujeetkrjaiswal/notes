# Command Injection

### What

A Command Injection vulnerability, when exploited by a malicious user, results in execution of arbitrary system commands on the host operating system. Command Injection attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers, etc) to a system command. The malicious system command is run server side with the same privileges as the application.

### How to avoid

1. Avoid `eval()`, `setTimeout()` and `setInterval()`
   1. setTimeout in browser, accepts string literals. `setTimeout("console.log(1+1)", 1000);`
2. Avoid `new Function()`
3. Avoid code serialization in JavaScript
4. Use a Node.js security linter
5. Use a static code analysis (SCA) tool to find and fix code injection issues

### Example

```javascript
const { exec } = require("child_process");

function executeCommand(username) { // username is received from user in a request
    exec(`some-command -${username}`, (error, stdout, stderr) => {
        if (error) {
            console.log(`error: ${error.message}`);
            return;
        }
        if (stderr) {
            console.log(`stderr: ${stderr}`);
            return;
        }
        console.log(`stdout: ${stdout}`);
    });
}
```

Since the `username` is being received from client application and without doing any  validation, it is passed directly to `exec` command. So the user can pass anything and can execute whatever command with the same previleges as that of the application.
