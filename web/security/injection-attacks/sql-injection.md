# SQL Injection



SQL injection attack occurs when:

1. An unintended data enters a program from an untrusted source.
2. The data is used to dynamically construct a SQL query

```typescript
const sqlQuery = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
```

### Solution: Use Prepared Statement

```javascript
try {
 const name = `GET NAME`
 const password = "GET PASSWORD"
 const query = "SELECT * from Users where Name=@name AND Pass=@password"
 const ps = new mssql.PreparedStatement(pool);
 ps.input('name', mssql.VarChar(50))
 ps.input('password', mssql.VarChar(50))
 await ps.prepare(stmt)
 await ps.execute({
  name,
  password
 })
 await ps.unprepare();
 // send response
} catch (e) {
 // handle error
}
```
