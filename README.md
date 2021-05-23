# MySQL(MariaDB)

## 설치
### Mac
* https://mariadb.com/kb/ko/installing-mariadb-on-macos-using-homebrew
```sh
# 설치
brew install mariadb

# DB 실행 (재시작 하여도 DB 자동 실행)
brew services start mariadb
# DB 종료 (재시작 하면 DB를 실행하지 않는다)
brew services stop mariadb
# DB 1회성 실행 (재시작과 상관없이 1회성으로 DB를 실행한다)
mysql.server start
```

## mysql 프로그램 실행
```sh
# DB 접속 프로그램 실행
mysql
  # 현재 사용자 계정의로 접속
sudo mysql -u root
  # 관리자 계정으로 접속
```

### 현재 접속한 사용자 보기
```mysql
select user();
# or
\s
```

### Mac용 mysql 접속 프로그램
* https://sequelpro.com/download
