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

## 1:N 상황
### left join
```sql
select *, (
  select count(*) from items i_sub where i_sub.member_pk = m.member_pk
) as count
from members m
left join items i on m.member_pk = i.member_pk;
```
* `count`를 이용해 `for문`을 돌려서 `JSON 형식`으로 만들어야 한다.

### JSON으로 출력 (JSON 형식의 문자로 출력됨)
* https://velog.io/@nextlinehappy516/Mysql-JSON-%ED%98%95%ED%83%9C%EB%A1%9C-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0
```sql
select json_object(
  'member_pk', m.member_pk, 'name', m.name, 'age', m.age,
  'items', if(isnull(i.item_pk), '[]', json_arrayagg(
    json_object(
      'item_pk', i.item_pk, 'name', i.name
    )
  ))
) as members_items
from members m
left join items i on m.member_pk = i.member_pk
group by m.member_pk;
```
```json
{"member_pk": 1, "name": "홍길동", "age": 39, "items": "[{\"item_pk\": 1, \"name\": \"사과\"},{\"item_pk\": 2, \"name\": \"딸기\"}]"}
{"member_pk": 2, "name": "김삼순", "age": 33, "items": "[]"}
```
`for문`을 돌려서 `JSON 형식의 문자`를 `JSON 형식`으로 변환 해야 한다.

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
