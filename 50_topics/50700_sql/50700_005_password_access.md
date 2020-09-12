# Password Access

```sql
-- creating a user with password following the default password encryption
CREATE USER 'sequelpro'@'localhost' IDENTIFIED BY 'passw0rd';
-- updaing the password encryption to native from Strong Password Encryption (caching_sha2_password) to Legacy (mysql_native_password)
ALTER USER 'sequelpro'@'localhost' IDENTIFIED WITH mysql_native_password BY 'passw0rd';
-- Granting all privileges on some db to a given user
GRANT ALL PRIVILEGES ON your_db.* TO 'sequelpro'@'localhost'
```

