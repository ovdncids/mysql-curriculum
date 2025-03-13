# TABLE

## Database 정보 보기
```sql
# Database 목록 보기
show databases;
## 권한이 있는 Database만 보여준다.

# test Database 생성
create database test;
## test Database이 없을 경우 생성한다.
## create database test default character set utf8;

# test Database 사용
use test;

# test Database 가지고 있는 Table 목록 보기
show tables;
```

## 테이블 만들기(create table문)
```sql
create table users (
  user_pk int auto_increment primary key,
  name varchar(200) not null,
  age int null
) charset = utf8;

alter table users convert to character set utf8mb4;
-- utf8mb4는 이모지(🙂)도 저장 가능하다.
```

### 테이블 삭제(drop table문)
```sql
drop table users;
```

## 테이블 안에 데이터 CRUD(create, read, update, delete)
### 생성(insert into문)
```sql
insert into users(name, age) values('홍길동', 39);
insert into users(name, age) values('김삼순', null);
insert into users(name, age) values('홍명보', 44);
insert into users(name, age) values('박지삼', 22);
insert into users(name, age) values('권명순', 10);
```
#### 마지막으로 생성된 primary key 얻기
```sql
select last_insert_id();
```

### 읽기(select from문)
```sql
select * from users;
```

### 삭제(delete from문)
```sql
delete from users where user_pk = 2;
delete from users;
```
* ❕ 중요: 이전 버전의 MySQL에서는 where문 없이 실행 가능 했다. where문이 없을 경우 모든 데이터가 삭제 된다.

### 수정(update문)
```sql
update users set age = 33 where user_pk = 2;
update users set age = 33 where 1 = 1;
update users set age = 33;
```
* ❔ `name`과 `age` 둘다 수정 하려면

### 테이블 안에 데이터 전부 삭제 및 초기화(drop table문)
```sql
truncate table users;
```
* ❕ 중요: truncate table을 사용할 경우 user_pk번호까지 초기화 된다.

### 테이블 복사(as문)
```sql
create table users_copy as (
  select * from users
);
```
* ❕ 중요: 하지만 primary key까지 복사 되지 않는다.

### 다른 테이블의 조회 결과를 현재 테이블에 넣기
```sql
insert into users_copy (
  select * from users
);
```

## 데이터가 없을 경우 생성문, 있을 경우 수정문
* https://stackoverflow.com/questions/4205181/insert-into-a-mysql-table-or-update-if-exists
```sql
insert into users(user_pk, name, age)
values (1, '홍길동', 39)
on duplicate key update
age = 40;
```

```sql
insert into users(user_pk, name, age)
values (1, '홍길동', 39), (1, '김삼순', 33)
on duplicate key update
age = values(age);
```
