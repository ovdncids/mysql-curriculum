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
## `Grant_priv 권한`만 들어가지 않는다.

# 특정 IP 대역 허용
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.10.0.%' IDENTIFIED BY '패스워드';

# IP 삭제
DELETE FROM mysql.user WHERE Host='%' AND User='root';
FLUSH PRIVILEGES;
```

## 사용자 권한
```sql
# Database 생성
CREATE DATABASE test DEFAULT CHARACTER SET utf8;

# 사용자 생성과 동시에 Database 사용권한 주기
GRANT ALL PRIVILEGES ON test.* TO 'test'@'localhost' IDENTIFIED BY '패스워드';
## Access denied for user 'root'@'%' to database 'tiara'
## 'root'@'%'는 `Grant_priv 권한`이 없을 수 있으므로 'root'@'localhost'으로 로그인 하거나 권한을 줘야 한다.
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
## `Grant_priv 권한`은 `FLUSH PRIVILEGES;`으로 적용 되지 않으므로 MySQL 서버를 재시작 한다.

# 사용자만 생성
CREATE USER 'test'@'localhost' IDENTIFIED BY '패스워드';
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
