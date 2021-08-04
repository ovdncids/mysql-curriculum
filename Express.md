# Express with MySQL

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

db.query('select ? as ?', [123, 'abc'], function (error, rows) {
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

1. `디버깅 모드`에서 확인

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
app.use('/api/v1/groceries', require('./routes/groceries.js'));
```

## groceries 라우터 생성
routes/groceries.js

### Create
```js
const router = global.express.Router();
const db  = global.db;

router.post('/', function(request, response) {
  const sql = `
    insert into groceries(member_pk, name, enter, expire)
    values (
      1,
      ?,
      date_format(now(), '%Y-%m-%d'),
      date_format(date_add(now(), interval + 2 week), '%Y-%m-%d')
    );
  `;
  db.query(sql, [request.body.name], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done groceries post', rows);
      response.status(200).send({
        result: 'Created'
      });
    }
  });
});
```
* `Swagger`에 `Create` 생성 하기

### Read
```js
router.get('/', function(request, response) {
  const orderName = request.query.orderName || 'name';
  const orderType = request.query.orderType || 'asc';
  const sql = `
    select * from groceries
    where member_pk = 1
    order by ? ?;
  `;
  db.query(sql, [orderName, orderType], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done groceries get', rows);
      response.status(200).send({
        result: 'Readed',
        groceries: rows
      });
    }
  });
});
```
* ❕ 정렬이 안된는 이유 설명

### Update
```js
router.patch('/', function(request, response) {
  const sql = `
    update groceries
    set expire = ?
    where grocery_pk = ? and member_pk = 1;
  `;
  db.query(sql, [request.body.grocery.expire, request.body.grocery_pk], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done groceries patch', rows);
      response.status(200).send({
        result: 'Updated'
      });
    }
  });
});
```

### Delete
```js
router.delete('/:grocery_pk', function(request, response) {
  const sql = `
    delete from groceries
    where grocery_pk = ? and member_pk = 1;
  `;
  db.query(sql, [request.params.grocery_pk], function (error, rows) {
    if (!error || db.error(request, response, error)) {
      console.log('Done groceries delete', rows);
      response.status(200).send({
        result: 'Deleted'
      });
    }
  });
});
```
