# Horizontal Privilege Escalation

s

```javascript
var sql = require("mssql");
​
app.get('/editProfile', function (req, res, next) {
    var uid = req.params.uid,
        ps = new sql.PreparedStatement(),
        sqlQuery = "select * from users where userid = @userid";
​
    ps.input('userid', sql.VarChar(50));
    ps.prepare(sqlQuery)
        .then(function () {
            return ps.execute({userid: uid}).then(function (recordset) {
                if (recordset.length > 0) {
                    res.render('editProfile', recordset[0]); //render the edit profile page.
                } else {
                    next(new Error('User Not Found'));
                }
            })
        })
        .catch(next);
​
});
​
```



```javascript
var sql = require("mssql");
​
app.get('/editProfile', function (req, res, next) {
    var uid = req.params.uid,
        ps = new sql.PreparedStatement(),
//        sqlQuery = "select * from users where userid = @userid";
        userName = req.user.name,
        sqlQuery = "select * from users where userid = @userid and username= @username";
​
​
    ps.input('userid', sql.VarChar(50));
    ps.input('username', sql.VarChar(50));
    ps.prepare(sqlQuery)
        .then(function () {
            return ps.execute({userid: uid, username: userName}).then(function (recordset) {
//            return ps.execute({userid: uid}).then(function (recordset) {
                if (recordset.length > 0) {
                    res.render('editProfile', recordset[0]); //render the edit profile page.
                } else {
                    next(new Error('User Not Found'));
                }
            })
        })
        .catch(next);
​
});
​
```
