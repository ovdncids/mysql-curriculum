# 권한

## ER_NOT_SUPPORTED_AUTH_MODE
* https://1mini2.tistory.com/88
```sql
USE mysql;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;

SELECT Host,User,plugin,authentication_string FROM mysql.user;
-- caching_sha2_password -> mysql_native_password 변경되었는지 확인
```
