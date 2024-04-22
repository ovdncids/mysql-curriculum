# 권한
## 외부 접속 허용
* [my.cnf 위치](https://docs.3rdeyesys.com/database/ncloud_database_mysql_mariadb_config_my_cnf.html#mysql)
```sh
# Mac - MySQL
vi /opt/homebrew/etc/my.cnf

# Mac - MariaDB
vi /opt/homebrew/etc/my.cnf.default

# Ubuntu, Raspberry Pi
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
```sh
# bind-address = 127.0.0.1
## 주석 처리 또는
bind-address = 0.0.0.0
```

### DBeaver에서 Public Key Retrieval is not allowed 발생하는 경우
```sh
Connection 창 > Driver properties > allowPublicKeyRetrieval > True
```

## 새로운 root 계정에 모든 IP, 모든 DB 사용 권한 허용
* https://java119.tistory.com/61
```sql
# 권한 확인
SELECT Host, User, plugin, authentication_string FROM mysql.user;

# 새로운 root 계정에 모든 IP 허용 ('root'@'localhost'와 별도로 생성해야 한다.)
CREATE USER 'root'@'%' identified by '패스워드';

# 새로운 root 계정에 모든 DB 사용 권한 허용 (하지만 `Grant_priv 권한`(GRANT 명령을 사용하는 권한)만 들어가지 않는다.)
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
## 'root'@'172.10.0.%' = 특정 IP 대역 허용

# 새로운 root 계정에 권한을 주는 권한 허용
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
## `Grant_priv 권한`은 `FLUSH PRIVILEGES;`으로 적용 되지 않으므로 MySQL 서버를 재시작 한다.

# 새로운 root 계정 삭제
DELETE FROM mysql.user WHERE Host='%' AND User='root';

# 권한 즉시 적용
FLUSH PRIVILEGES;
```

## test 사용자에게 test DB 사용 권한 허용
```sql
# test DB 생성
CREATE DATABASE test DEFAULT CHARACTER SET utf8mb4;

# test 개정 생성
CREATE USER 'test'@'%' identified by '패스워드';
# test 패스워드 변경
GRANT usage on *.* to 'test'@'%' identified by '패스워드';

# test 개정에 test DB 사용 권한 허용
GRANT ALL PRIVILEGES ON test.* TO 'test'@'%';
```

## 권한 실수로 MariaDB 완전 삭제
* https://geondev.github.io/mariadb-db-remove-m1

# 백업
```sh
DBeaver > Databases > {백업할 DB} > 도구 > Dump database > DB와 Table 선택 후 다음 > Output folder 선택 후 Start
```
