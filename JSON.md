# MySQL with JSON
* https://mariadb.com/kb/en/differences-between-json_query-and-json_value

# MS-SQL with JSON
```sql
WITH AA AS (SELECT *
    FROM OpenJson('[{"town":"Belgrade"},{"town":"Paris"},{"town":"Madrid"}]') WITH (TOWN varchar(20) '$.town')
) SELECT * FROM AA;
```
