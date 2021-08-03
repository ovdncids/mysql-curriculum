# Express with MySQL

## mysql connection 설치
```sh
npm install mysql
```

mysql-connection.js
```js
const mysql = require('mysql');

const connection = mysql.createPool({
  host: 'localhost',
  user: 'user',
  password: 'password',
  database: 'test'
});

const error = function (request, ressponse, error) {
  console.error('Start: SQL error');
  console.error(error.sql);
  console.error('End: SQL error');
  // error.sql = undefined;
  ressponse.status(500).send(error);
  return false;
};

module.exports = {
  query: connection.query,
  error: error
};

connection.query('select 123 as abc', null, function (error, rows) {
  console.log(error);
  console.log(rows);
});
```

```diff
- connection.query('select 123 as abc', null, function (error, rows) {
+ connection.query('select ? as abc', [123], function (error, rows) {
```


