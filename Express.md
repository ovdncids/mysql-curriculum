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

db.query('select 123 as abc', null, function(error, rows) {
  console.error(error);
  console.log(rows);
});

db.error = function(req, res, error) {
  console.error('Start: SQL error');
  console.error(error.sql);
  console.error('End: SQL error');
  // error.sql = undefined;
  res.status(500).send(error);
  return false;
};

module.exports = db;
```

1. `mysql-connector.js` 파일을 `디버깅 모드`에서 확인

```diff
- db.query('select 123 as abc', null, function(error, rows) {
+ db.query('select ? as ?', [123, 'abc'], function(error, rows) {
```

2. 쿼리문에 `?` 사용
* <details><summary>환경설정</summary>

  ```js
  const connections = {
    product: {
      host: 'localhost',
      user: 'user',
      password: 'password',
      database: 'test'
    },
    dev: {
      host: 'localhost',
      user: 'user',
      password: 'password',
      database: 'test'
    }
  };
  process.env.NODE_ENV = 'product';
  if (process.env.USER === 'ec2-user') {
    process.env.NODE_ENV = 'dev';
  }
  const db = mysql.createPool(connections[process.env.NODE_ENV]);
  ```
</details>

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
const db = global.db;

router.post('/', function(req, res) {
  const sql = `
    insert into items(user_pk, name, enter, expire)
    values (
      1,
      ?,
      date_format(now(), '%Y-%m-%d'),
      date_format(date_add(now(), interval + 2 week), '%Y-%m-%d')
    );
  `;
  db.query(sql, [req.body.name], function(error, rows) {
    if (error) return db.error(req, res, error);
    console.log('Done items post', rows);
    res.status(200).send({
      result: 'Created'
    });
  });
});

module.exports = router;
```
* `Swagger`에 `Create` 생성 하기

### Read
```js
router.get('/', function(req, res) {
  const orderName = req.query.orderName || 'name';
  const orderType = req.query.orderType || 'asc';
  const sql = `
    select * from items
    where user_pk = 1
    order by ? ?;
  `;
  db.query(sql, [orderName, orderType], function(error, rows) {
    if (error) return db.error(req, res, error);
    console.log('Done items get', rows);
    res.status(200).send({
      result: 'Readed',
      items: rows
    });
  });
});
```
* ❕ 정렬이 안된는 이유 설명

### Delete
```js
router.delete('/:items_pk', function(req, res) {
  const sql = `
    delete from items
    where items_pk = ? and user_pk = 1;
  `;
  db.query(sql, [req.params.items_pk], function(error, rows) {
    if (error) return db.error(req, res, error);
    console.log('Done items delete', rows);
    res.status(200).send({
      result: 'Deleted'
    });
  });
});
```

### Update
```js
router.patch('/:items_pk', function(req, res) {
  const sql = `
    update items
    set expire = ?
    where items_pk = ? and user_pk = 1;
  `;
  db.query(sql, [req.body.expire, req.params.items_pk], function(error, rows) {
    if (error) return db.error(req, res, error);
    console.log('Done items patch', rows);
    res.status(200).send({
      result: 'Updated'
    });
  });
});
```

* Frontend에 적용

### like문
```js
router.get('/', function(req, res) {
  const q = req.query.q || '';
  const sql = `
    select * from users
    where ? = '' or name like concat('%', ?, '%')
  `;
  db.query(sql, [q, q], function(error, rows) {
    if (error) return db.error(req, res, error);
    console.log('Done users search', rows);
    res.status(200).send({
      result: 'Searched',
      items: rows
    });
  });
});
```
* https://dongram.tistory.com/12

## Node MySQL 2 (Promise)
* https://github.com/sidorares/node-mysql2
```sh
npm install mysql2
```
```js
const main = async function() {
  const mysql = require('mysql2/promise');
  const connection = await mysql.createConnection({
    host: 'localhost',
    user: 'user',
    password: 'password',
    database: 'test'
  });
  const [rows, fields] = await connection.execute(`
    select * from users where name = ? and age = ?
  `, ['홍길동', 39]);
};
```

## ER_NOT_SUPPORTED_AUTH_MODE
* https://1mini2.tistory.com/88
```sql
USE mysql;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
FLUSH PRIVILEGES;

SELECT Host,User,plugin,authentication_string FROM mysql.user;
-- caching_sha2_password -> mysql_native_password 변경 되었는지 확인
```
