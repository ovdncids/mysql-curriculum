# EXPLAIN (실행 계획)
```sql
# 기본적인 실행계획
EXPLAIN SELECT * FROM users;

# JSON 형식 실행계획
EXPLAIN FORMAT=JSON SELECT * FROM users;

# 확장 (버전에 따라 안 될 수 있음)
EXPLAIN EXTENDED SELECT * FROM users;
```
