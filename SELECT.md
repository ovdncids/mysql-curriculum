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
* Export 해보기

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
select members.name from (
  /* SubQuery */
  select '홍길동' as name, 39 as age
) members;
```
* 모든 열 보기
```diff
- members.name
+ members.*
```
* ❔ 문제2: members를 문제1과 같이 출력 하려면

## 검색 하기(where문)
* if문이라고 생각하면 쉽다

### SubQuery와 관계 없는 조건 사용하기
```sql
select members.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) members
where 1 = 1;
```
* 거짓 조건 사용하기
```diff
- where 1 = 1;
+ where 1 = 2;
```

### SubQuery와 관계 있는 조건 사용하기
```sql
select members.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) members
where members.name = '홍길동';
```
* 조건 추가 하기(and문 또는 or문)
```diff
- where members.name = '홍길동';
```
```sql
where members.name = '홍길동' and members.age = 39;
where members.name = '홍길동' or members.age = 33;
where 1 = 1 or (
  members.name = '홍길동' or members.age = 33
);
```
* 조건 상세 추가 하기(like문 또는 in문)
```sql
where members.name like '%홍%';
where members.name in ('홍길동', '박지삼');
```

## 정렬 하기(order by문)
```sql
select members.* from (
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
) members
order by members.name;
```
* 정렬 추가 하기(asc문, desc문)
```diff
- order by members.name;
```
```sql
order by members.name, members.age desc;
order by members.name desc, members.age desc;
```

## 출력 개수 정하기(limit문)
```sql
select members.* from (
  select '홍길동' as name, 39 as age
  union all
  select '김삼순', 33
  union all
  select '홍명보', 44
  union all
  select '박지삼', 22
  union all
  select '권명순', 10
) members
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
order by members.name
limit 0, 10;
```
