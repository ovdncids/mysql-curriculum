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
* https://dev.mysql.com/downloads/workbench
```mysql
Server name: localhost
Login: user@localhost
```

## select문과 친해지기
### 1행(row) 출력
| 제목1 | 제목2 | 제목3 |
|---|:---|:---|
| 1 | 2 | 삼 |
```sql
# 1, '2', '삼' 출력
select 1, '2', '삼';
# 열(column) 제목 넣기
select 1 as One, '2' as Two, '삼' as Three;
```

### 2행(row) 출력
| 제목1 | 제목2 | 제목3 |
|---|:---|:---|
| 1 | 2 | 삼 |
| four | 오 | 6 |
```sql
select 1 as 제목1, '2' as 제목2, '삼' as 제목3
union
select 'four', '오', 6;
```

* ❕ 중요: 동일한 행을 2번 쓴다면
  ```sql
  select 1 as 제목1, '2' as 제목2, '삼' as 제목3
  union
  select 'four', '오', 6
  union
  select 'four', '오', 6;
  ```
  ```sql
  select 1 as 제목1, '2' as 제목2, '삼' as 제목3
  union all
  select 'four', '오', 6
  union all
  select 'four', '오', 6;
  ```

* ❔ 문제1: 다음과 같이 만들기
  | name | age |
  |---|:---|
  | 홍길동 | 39 |
  | 김삼순 | 33 |
  | 홍명보 | 44 |
  | 박지삼 | 22 |
  | 권명순 | 10 |
* <details><summary>정답</summary>

  ```sql
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10;
  ```
</details>

### 필요한 열만 보기
```sql
select members.name from (
  select '홍길동' as name, 39 as age
) members;
```
* 모든 열 보기
```diff
- members.name
+ members.*
```
* ❔ 문제2: members를 문제1과 같이 추가 하려면

### 검색 하기
