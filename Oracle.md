# Oracle

## Oracle SQL Developer
* https://www.oracle.com/tools/downloads/sqldev-downloads.html

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
