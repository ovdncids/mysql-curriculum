# JOIN

## date 관련 함수
```sql
# 현재 날짜
select now();

# 현재 날짜에 2주 더하기
select date_add(now(), interval + 2 week);
  # microsecond, second, minute, hour, day, week, month(3월31일 - 1 이면 2월 28일. 윤년이면 29일),
  # quarter(1년의 4분기 1 ~ 4까지 사용가능), year

# 현재 날짜 포맷 마추기
select date_format(now(), '%Y-%m-%d %H:%i:%S');
```

## groceries 테이블 만들기
```sql
create table groceries (
  pk int auto_increment primary key,
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

* ❔ 문제1: `2021-01-01`를 `date_format`함수를 사용하여 insert문 완성하기
* ❔ 문제2: `2021-01-15`를 `date_format`함수와 `date_add`함수를 사용하여 insert문 완성하기
* <details><summary>정답</summary>

  ```sql
  insert into groceries(member_pk, name, enter, expire)
  values (1, '사과', date_format(now(), '%Y-%m-%d'), date_format(date_add(now(), interval + 2 week), '%Y-%m-%d'));
  ```
</details>
