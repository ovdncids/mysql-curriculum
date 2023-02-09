# SELECT
## 출력 하기(select문)
### 1행(row) 출력
| One | Two | Three |
|---|:---|:---|
| 1 | 2 | 삼 |
```sql
# 1, '2', '삼' 출력
select 1, '2', '삼';
# 열(column) 제목 넣기
select 1 as One, '2' as Two, '삼' as Three;
```

### 2행(row) 출력(union문)
| 제목1 | 제목2 | 제목3 |
|---|:---|:---|
| 1 | 2 | 삼 |
| four | 오 | 6 |
```sql
select 1 as 제목1, '2' as 제목2, '삼' as 제목3
union
select 'four', '오', 6;
```
* 쿼리 실행 후 결과 하단에 `데이터 추출...` 해보기

* ❕ 중요: 동일한 행을 2번 쓴다면
  ```sql
  select 1 as 제목1, '2' as 제목2, '삼' as 제목3
  union
  select 'four', '오', 6
  union
  select 'four', '오', 6;
  ```
  ```sql
  select 1 as 제목1, '2' as 제목2, '삼' as 제목3
  union all
  select 'four', '오', 6
  union all
  select 'four', '오', 6;
  ```
  ***union문은 위치와 관계 없이 중복된 행을 제거 한다***

* ❔ 문제1: 다음과 같이 만들기
  | name | age |
  |---|:---|
  | 홍길동 | 39 |
  | 김삼순 | 33 |
  | 홍명보 | 44 |
  | 박지삼 | 22 |
  | 권명순 | 10 |
* <details><summary>정답</summary>

  ```sql
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10;
  ```
</details>

### 필요한 열만 보기(중첩 select문 또는 SubQuery)
```sql
# MainQuery
select users.name from (
  /* SubQuery */
  select '홍길동' as name, 39 as age
) users;
```
* 모든 열 보기
```diff
- users.name
+ users.*
```
* ❔ 문제2: users를 문제1과 같이 출력 하려면

## 검색 하기(where문)
* if문이라고 생각하면 쉽다

### SubQuery와 관계 없는 조건 사용하기
```sql
select users.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) users
where 1 = 1;
```
* 거짓 조건 사용하기
```diff
- where 1 = 1;
+ where 1 = 2;
```

### SubQuery와 관계 있는 조건 사용하기
```sql
select users.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) users
where users.name = '홍길동';
```
* 조건 추가 하기(and문 또는 or문)
```diff
- where users.name = '홍길동';
```
```sql
where users.name = '홍길동' and users.age = 39;
where users.name = '홍길동' or users.age = 33;
where 1 = 1 or (
  users.name = '홍길동' or users.age = 33
);
```
* 조건 상세 추가 하기(like문 또는 in문)
```sql
where users.name like '%홍%';
where users.name in ('홍길동', '박지삼');
```

## 정렬 하기(order by문)
```sql
select users.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '박지삼', 11
  union all
  select '권명순', 10
  union all
  select '권명순', 20
) users
order by users.name;
```
* 정렬 추가 하기(asc문, desc문)
```diff
- order by users.name;
```
```sql
order by users.name, users.age desc;
order by users.name desc, users.age desc;
```

## 출력 개수 정하기(limit문)
```sql
select users.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) users
limit 0, 10;
  # 첫 레코드는 0부터 시작 된다
  # 첫 레코드 부터 10개를 보여준다
```
* 출력 위치와 개수 수정 하기
```diff
- limit 0, 10;
```
```sql
limit 2, 1;
limit 4, 2;
```

### 검색, 정렬, 출력 동시에 사용하기
```sql
where 1 = 1
order by users.name
limit 0, 10;
```
