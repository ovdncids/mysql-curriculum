# MySQL with JSON
* https://mariadb.com/kb/en/json_table
```sql
set @json='
[
  {"name":"홍길동", "age": 39},
  {"name":"김삼순", "age": 33},
  {"name":"홍명보", "age": 44},
  {"name":"박지삼", "age": 22},
  {"name":"권명순", "age": 10}
]
';
select *
from json_table(@json, '$[*]' columns(
  name  nvarchar(200) path '$.name',
  age int path '$.age'
)) as members;
```

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
);
```
