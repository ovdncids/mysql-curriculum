# GROUP_CONCAT
```sql
select sum(A), group_concat(B separator ', ')
from (
  select 123 as A, 'abc' as B
  union all
  select 321, 'cba'
) T
```
