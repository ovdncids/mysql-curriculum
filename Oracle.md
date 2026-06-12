# Oracle

## Oracle SQL Developer
* https://www.oracle.com/tools/downloads/sqldev-downloads.html

## Docker
* https://hub.docker.com/r/gvenzl/oracle-free?uuid=85375817-eaf6-4776-8a29-b39336fba14b
```sh
docker run --name oracle -d -p 1521:1521 -e ORACLE_PASSWORD=1234 gvenzl/oracle-free:23.26.2

# Host: localhost
# Database: FREEPDB1
# Username: system

# 버전 확인
SELECT * FROM v$version;
```

### 사용자 생성와 DB 생성
```sh
# Docker Desktop > Containers > oracle > Exec
sh-4.4$ (Shell 접속 완료)
createAppUser test 1234 FREEPDB1;

# SQL Plus
sqlplus test/1234@FREEPDB1

# 현재 접속한 사용자
SELECT USER FROM dual;

# 현재 접속한 DB
SELECT SYS_CONTEXT('USERENV','CON_NAME') AS CURRENT_PDB FROM dual;

# Table 생성
CREATE TABLE USERS (
  NAME VARCHAR(200) NOT NULL,
  AGE INT NULL
);
```

## Mac 접속중 에러
status : failure -test failed: ora-00604: error occurred at recursive sql level 1 ora-01756: quoted string not properly terminated
* https://proni.tistory.com/entry/%E2%9C%85-Solved-oracle-sqldeveloper-%EC%97%B0%EA%B2%B0-%EC%8B%9C-%EC%97%90%EB%9F%AC-at-Mac
```sh
시스템 환경설정 > 언어 및 지역
  English 최상단으로 이동
  지역: 미국

Oracle SQL Developer > 재실행 후 재접속
SQL Developer > Oracle 접속 후 > 언어 및 지역 복구
```

## 해당 SCHEMA로 이동
```sql
# use 디비명 (동일)
ALTER SESSION SET CURRENT_SCHEMA = 스키마명;
```
