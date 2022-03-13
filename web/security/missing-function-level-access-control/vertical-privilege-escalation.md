# Vertical Privilege Escalation



```javascript

app.get('/getAllUsers',
    function (req, res, next) {  //middleware that ensures the user is authenticated
        if (req.isAuthenticated()) {
            return next();
        }
​
        // denied. redirect to login
        res.redirect('/login');
​
    },
    function (req, res, next) {
        var users = User.find() //mongodb get all users.
        res.render('admin_show_users', users);
    }
);
​
## FIXED
​
app.get('/getAllUsers',
    function (req, res, next) {  //middleware that ensures the user is authenticated
        if (req.isAuthenticated()) {
            return next();
        }
​
        // denied. redirect to login
        res.redirect('/login');
​
    },
​
    function (req, res, next) { //verify user has permission to view this page
        if (req.user.role !== 'admin') {
            res.status(403).send('Access Denied');
        } else {
            next();
        }
    },      
  
    function (req, res, next) {
        var users = User.find() //mongodb get all users.
        res.render('admin_show_users', users);
    }
);
​
```
