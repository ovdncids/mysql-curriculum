# 권한
## 외부 접속 허용
* [my.cnf 위치](https://docs.3rdeyesys.com/database/ncloud_database_mysql_mariadb_config_my_cnf.html#mysql)
```sh
# Mac
vi /opt/homebrew/etc/my.cnf.default

# Raspberry Pi
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
```sh
# bind-address = 127.0.0.1
## 주석 처리 또는
bind-address = 0.0.0.0
```

## 사용자 IP 허용
* https://java119.tistory.com/61
```sql
# 권한 확인
SELECT Host, User, plugin, authentication_string FROM mysql.user;

# 모든 IP 허용
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '패스워드';

# 특정 IP 대역 허용
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.10.0.%' IDENTIFIED BY '패스워드';

# IP 삭제
DELETE FROM mysql.user WHERE Host='%' AND User='root';
FLUSH PRIVILEGES;
```

## Node - ER_NOT_SUPPORTED_AUTH_MODE
* https://1mini2.tistory.com/88
```sql
USE mysql;

SELECT Host, User, plugin, authentication_string FROM mysql.user;
# caching_sha2_password -> mysql_native_password 변경되었는지 확인

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;
```
