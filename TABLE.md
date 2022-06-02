# TABLE

## Database 정보 보기
```sql
# Database 목록 보기
show databases;
  # 권한이 있는 Database를 보여준다.
create database test;
  # test Database이 없을 경우 생성한다.
use test;
  # test Database에 접근한다.
show tables;
  # test Database가 가지고 있는 Table을 보여준다.
```

## 테이블 만들기(create table문)
```sql
create table members (
  member_pk int auto_increment primary key,
  name nvarchar(200) not null,
  age int null
);
```

### 테이블 삭제(drop table문)
```sql
drop table members;
```

## 테이블 안에 데이터 CRUD(create, read, update, delete)
### 생성(insert into문)
```sql
insert into members(name, age) values('홍길동', 39);
insert into members(name, age) values('김삼순', null);
insert into members(name, age) values('홍명보', 44);
insert into members(name, age) values('박지삼', 22);
insert into members(name, age) values('권명순', 10);
```
#### 마지막으로 생성된 primary key 얻기
```sql
select last_insert_id();
```

### 읽기(select from문)
```sql
select * from members;
```

### 수정(update문)
```sql
update members set age = 33 where member_pk = 2;
update members set age = 33 where 1 = 1;
update members set age = 33;
```
* ❕ 중요: 이전 버전의 MySQL에서는 where문 없이 실행 가능 했다. where문이 없을 경우 모든 데이터가 수정 된다.

### 삭제(delete from문)
```sql
delete from members where member_pk = 2;
delete from members;
```

### 테이블 안에 데이터 전부 삭제 및 초기화(drop table문)
```sql
truncate table members;
```
* ❕ 중요: truncate table을 사용할 경우 member_pk번호까지 초기화 된다.

### 테이블 복사(as문)
```sql
create table members_copy as (
  select * from members
);
```
* ❕ 중요: 하지만 primary key까지 복사 되지 않는다.

### 다른 테이블의 조회 결과를 현재 테이블에 넣기
```sql
insert into members_copy (
  select * from members
);
```

## 데이터가 없을 경우 생성문, 있을 경우 수정문
```sql
insert into members(member_pk, name, age) values (1, '홍길동', 39)
on duplicate key update
age = 40;
```
