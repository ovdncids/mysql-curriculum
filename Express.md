# Express with MySQL
* [Download](https://github.com/ovdncids/vue-curriculum/raw/master/download/express-server.zip)

## mysql 라이브러리 설치
```sh
npm install mysql
```

mysql-connector.js
```js
const mysql = require('mysql');

const db = mysql.createPool({
  host: 'localhost',
  user: 'user',
  password: 'password',
  database: 'test'
});

db.query('select 123 as abc', null, function (error, rows) {
  console.log(error);
  console.log(rows);
});

db.error = function (request, ressponse, error) {
  console.error('Start: SQL error');
  console.error(error.sql);
  console.error('End: SQL error');
  // error.sql = undefined;
  ressponse.status(500).send(error);
  return false;
};

module.exports = db;
```

1. `mysql-connector.js` 파일을 `디버깅 모드`에서 확인

```diff
- db.query('select 123 as abc', null, function (error, rows) {
+ db.query('select ? as ?', [123, 'abc'], function (error, rows) {
```

2. 쿼리문에 `?` 사용

## index.js에서 부르기
index.js
```js
// MySQL
global.db = require('./mysql-connector.js');
```
```js
app.use('/api/v1/items', require('./routes/items.js'));
```

## items 라우터 생성
routes/items.js

### Create
```js
const router = global.express.Router();
const db  = global.db;

router.post('/', function(request, response) {
  const sql = `
    insert into items(member_pk, name, enter, expire)
    values (
      1,
      ?,
      date_format(now(), '%Y-%m-%d'),
      date_format(date_add(now(), interval + 2 week), '%Y-%m-%d')
    );
  `;
  db.query(sql, [request.body.name], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done items post', rows);
      response.status(200).send({
        result: 'Created'
      });
    }
  });
});

module.exports = router;
```
* `Swagger`에 `Create` 생성 하기

### Read
```js
router.get('/', function(request, response) {
  const orderName = request.query.orderName || 'name';
  const orderType = request.query.orderType || 'asc';
  const sql = `
    select * from items
    where member_pk = 1
    order by ? ?;
  `;
  db.query(sql, [orderName, orderType], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done items get', rows);
      response.status(200).send({
        result: 'Readed',
        items: rows
      });
    }
  });
});
```
* ❕ 정렬이 안된는 이유 설명

### Delete
```js
router.delete('/:items_pk', function(request, response) {
  const sql = `
    delete from items
    where items_pk = ? and member_pk = 1;
  `;
  db.query(sql, [request.params.items_pk], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done items delete', rows);
      response.status(200).send({
        result: 'Deleted'
      });
    }
  });
});
```

### Update
```js
router.patch('/:items_pk', function(request, response) {
  const sql = `
    update items
    set expire = ?
    where items_pk = ? and member_pk = 1;
  `;
  db.query(sql, [request.body.expire, request.params.items_pk], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done items patch', rows);
      response.status(200).send({
        result: 'Updated'
      });
    }
  });
});
```

## Frontend에 적용

<!--
## ER_NOT_SUPPORTED_AUTH_MODE
* https://1mini2.tistory.com/88
```sql
USE mysql;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;

SELECT Host,User,plugin,authentication_string FROM mysql.user;
-- caching_sha2_password -> mysql_native_password 변경 되었는지 확인
```
-->
