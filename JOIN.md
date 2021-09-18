# JOIN

## date 관련 함수
```sql
# 현재 날짜
select now();

# 현재 날짜에 2주 더하기
select date_add(now(), interval + 1 week);
  # microsecond, second, minute, hour, day, week, month(3월31일 - 1 이면 2월 28일. 윤년이면 29일),
  # quarter(1년의 4분기 1 ~ 4까지 사용가능), year

# 현재 날짜 포맷 마추기
select date_format(now(), '%Y-%m-%d %H:%i:%S');
```

## groceries 테이블 만들기
```sql
create table groceries (
  grocery_pk int auto_increment primary key,
  member_pk int not null,
  name nvarchar(200) not null,
  enter date not null,
  expire date not null
);
```

### groceries 테이블에 데이터 넣기
```sql
insert into groceries(member_pk, name, enter, expire)
values (1, '사과', '2021-01-01', '2021-01-15');
```

* ❔ 문제1: `2021-01-01`를 `date_format`함수를 사용하여 `insert into`문 완성하기
* ❔ 문제2: `2021-01-15`를 `date_format`함수와 `date_add`함수를 사용하여 `insert into`문 완성하기
* <details><summary>정답</summary>

  ```sql
  insert into groceries(member_pk, name, enter, expire)
  values (1, '사과', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  ```
  추가 데이터 넣기
  ```sql
  insert into groceries(member_pk, name, enter, expire)
  values (1, '딸기', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  insert into groceries(member_pk, name, enter, expire)
  values (2, '바나나', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  insert into groceries(member_pk, name, enter, expire)
  values (3, '망고', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  insert into groceries(member_pk, name, enter, expire)
  values (100, '자몽', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  ```
</details>

## members 테이블과 groceries 테이블 조인 하기(join문)
```sql
select * from members m inner join groceries g on m.member_pk = g.member_pk;
select * from members m left outer join groceries g on m.member_pk = g.member_pk;
select * from members m right outer join groceries g on m.member_pk = g.member_pk;
```

## items 테이블 만들기
```sql
create table items (
  item_pk int primary key,
  member_pk int not null,
  name nvarchar(200) not null,
  enter date not null,
  expire date not null
);
```

### items 테이블에 groceries 테이블의 데이터 복사 하기
```sql
insert into items (
  select grocery_pk as item_pk, member_pk, name, enter, expire from groceries
);
```

* ❔ 문제: `items` 테이블의 `item_pk = 1`인 데이터만 삭제 하기
* <details><summary>정답</summary>

  ```sql
  delete from items where item_pk = 1;
  ```
</details>

* ❔ 문제: `groceries` 테이블의 `grocery_pk = 1`인 데이터만 `items` 테이블에 넣기
* <details><summary>정답</summary>

  ```sql
  insert into items (
    select grocery_pk as item_pk, member_pk, name, enter, expire from groceries
    where grocery_pk = 1
  );
  ```
  * ❕ 다시 한번 실행 하기
</details>

### groceries 테이블에서 복사된 데이터가 items 테이블에 있는지 확인
```sql
select *, (
  select item_pk from items i where i.item_pk = g.grocery_pk
) as item_pk
from groceries g;
```

<!--
### MySQL/MariaDB Table Update Safe 모드
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jevida&logNo=221123654036
```sql
SET SQL_SAFE_UPDATES = 0; --해제
SET SQL_SAFE_UPDATES = 1; --설정
```
-->
