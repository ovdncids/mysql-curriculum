# Express with MySQL

## mysql connection 설치
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
```js
// MySQL
global.db = require('./mysql-connector.js');
```

