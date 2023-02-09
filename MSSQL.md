# MS-SQL

## Microsoft SQL Server Management Studio
### 포트 설정
```sh
127.0.0.1,1433
```

## LAG, LEAD
* `LAG`: 이전 행의 데이터를 조회
* `LEAD`: 다음 행의 데이터를 조회
```sql
SELECT
  users.*
  , LAG(name, 1) OVER (ORDER BY name)
  , LEAD(name, 1, '') OVER (ORDER BY name)
  --, LAG(name, 1) OVER (PARTITION BY name ORDER BY name)
  --, LEAD(name, 1) OVER (PARTITION BY name ORDER BY name)
FROM (
  SELECT '홍길동' AS name, 39 AS age
  UNION ALL
  SELECT '김삼순', 33
  UNION ALL
  SELECT '홍명보', 44
  UNION ALL
  SELECT '박지삼', 22
  UNION ALL
  SELECT '권명순', 10
) users;
```
* `PARTITION BY`: `GROUP BY`라고 생각하면 쉽다.

## 마지막 PK 번호 넣기
* 일반 적인 경우
```sql
INSERT INTO users(user_pk, name, age) VALUES(
  (SELECT ISNULL(MAX(user_pk) + 1, 1) FROM users),
  '김유신',
  63
);
```

* bigint인 경우
```sql
INSERT INTO users(user_pk, name, age) VALUES(
  ISNULL((SELECT TOP 1 user_pk FROM users ORDER BY LEN(user_pk) DESC, user_pk DESC) + 1, 1),
  '김유신',
  63
);
```

## FOR JSON AUTO
```sql
SELECT
  users.*
FROM (
  SELECT '홍길동' AS name, 39 AS age
  UNION ALL
  SELECT '김삼순', 33
  UNION ALL
  SELECT '홍명보', 44
  UNION ALL
  SELECT '박지삼', 22
  UNION ALL
  SELECT '권명순', 10
) users
FOR JSON AUTO;
```
```json
[{"name":"홍길동","age":39},{"name":"김삼순","age":33},{"name":"홍명보","age":44},{"name":"박지삼","age":22},{"name":"권명순","age":10}]
```

## Date
```sql
SELECT FORMAT(CURRENT_TIMESTAMP, 'yyyy-MM-dd HH:mm:ss.fff');
SELECT CONVERT(DATETIME, '2022-01-02 11:22:33.777', 121);
```

# MSSQL-Paging

## MS-SQL 2000
```sql
SELECT IDENTITY(INT, 1, 1) AS ROWNUM, A.*
INTO #Temp
FROM (추출결과) A
SELECT * FROm #Temp
```

## MS-SQL 2005
```sql
SELECT * FROM (
  SELECT *
    , ROW_NUMBER() OVER (ORDER BY idx_num ASC) AS ROW_NUM            /* 몇 페이지의 몇 개의 레코드를 부를때 사용 */
    , ROW_NUMBER() OVER (ORDER BY idx_num DESC) AS ROW_NUM_REVERSE   /* 리스트에서 번호로 사용 */
    , COUNT(*) OVER() AS PG_CNT_
  FROM user_table
) A
WHERE ROW_NUM BETWEEN 1 AND 5 ORDER BY idx_num DESC
```
