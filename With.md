# 가상 테이블
```sql
with Z as (
  select * from (
    select '2022' as YY, '01' as MM, '101' as PK
    union all
    select '2022' as YY, '02' as MM, '101' as PK
    union all
    select '2022' as YY, '01' as MM, '102' as PK
    union all
    select '2022' as YY, '03' as MM, '102' as PK
    union all
    select '2022' as YY, '01' as MM, '103' as PK
  ) as A
)
select
  *
  , (
    select concat(YY, MM) from Z
    where Z.PK = Y.PK
    order by YY asc, MM asc
    limit 1
  ) as YYMM
from Z as Y
order by YYMM asc, PK desc, YY asc, MM asc;
```

| YY | MM | PK | YYMM |
|---|:---|:---|:---|
| 2022 | 01 | 103 | 202201 |
| 2022 | 01 | 102 | 202201 |
| 2022 | 03 | 102 | 202201 |
| 2022 | 01 | 101 | 202201 |
| 2022 | 02 | 101 | 202201 |

* `with문`은 서브쿼리를 가상 테이블로 정의 하고, 자신이 자신을 서브쿼리로 활용 할때 유용
