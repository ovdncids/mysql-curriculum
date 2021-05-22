# MySQL(MariaDB)

## 설치
### Mac
* https://mariadb.com/kb/ko/installing-mariadb-on-macos-using-homebrew
```sh
# 설치
brew install mariadb

# DB 실행
brew services start mariadb
# DB 종료
brew services stop mariadb

# DB 접속 프로그램 실행
mysql
  # 현재 사용자 계정의로 접속
sudo mysql -u root
  # 관리자 계정으로 접속
```

## mysql 프로그램 실행
```mysql
# 현재 접속한 사용자 보기
select user();
```
