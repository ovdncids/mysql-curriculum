# Paging
## MySQL
```sql
select * from users where 1 = 1 limit 0, 10;
select count(*) from users where 1 = 1;
```
* ❕ Frontend에서 `Page Navigation`을 만들기 위해서는 `레코드 수`가 필요 하므로 쿼리를 2번 실행 해야 한다.
* `SQL_CALC_FOUND_ROWS`, `FOUND_ROWS` 사용하는 방법도 있지만 `MySQL 8.0.17`부터 더 이상 사용되지 않는다.
* https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_found-rows

## MSSQL
* https://github.com/ovdncids/mysql-curriculum/blob/master/MSSQL.md#mssql-paging
