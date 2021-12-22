# MS-SQL
## LAG, LEAD
* `LAG`: 이전 행의 데이터를 조회
* `LEAD`: 다음 행의 데이터를 조회
```sql
select
  members.*
  , LAG(name, 1) OVER (ORDER BY name)
  , LEAD(name, 1, '') OVER (ORDER BY name)
  --, LAG(name, 1) OVER (PARTITION BY name ORDER BY name)
  --, LEAD(name, 1) OVER (PARTITION BY name ORDER BY name)
from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) members;
```
* `PARTITION BY`: `GROUP BY`라고 생각하면 쉽다.

## 마지막 PK 번호 넣기
* 일반 적인 경우
```sql
INSERT INTO members(member_pk, name, age) VALUES(
  (SELECT ISNULL(MAX(member_pk) + 1, 1) FROM members),
  '김유신',
  63
);
```

* bigint인 경우
```sql
INSERT INTO members(member_pk, name, age) VALUES(
  ISNULL((SELECT TOP 1 member_pk FROM members ORDER BY LEN(member_pk) DESC, member_pk DESC) + 1, 1),
  '김유신',
  63
);
```
