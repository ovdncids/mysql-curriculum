# MySQL with JSON
* https://mariadb.com/kb/en/differences-between-json_query-and-json_value

# MS-SQL with JSON
```sql
SELECT *
FROM
OpenJson('
[
  {"name":"홍길동", "age": 39},
  {"name":"김삼순", "age": 33},
  {"name":"홍명보", "age": 44},
  {"name":"박지삼", "age": 22},
  {"name":"권명순", "age": 10}
]
')
WITH (
  name nvarchar(200) '$.name',
  age int '$.age'
)
```
