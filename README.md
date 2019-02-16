### ioredis
---
https://github.com/luin/ioredis

```
npm install ioredis
```

```js
var Redis = require('ioredis');
var redis = new Redis();

redis.set('foo', 'bar');
redis.get('foo', function (err, result) {
  console.log(result);
});

redis.get('foo').then(function (result) {
  console.log(result);
});

redis.sadd('set', 1, 3, 5, 7);
redis.sadd('set', [1, 3, 5, 7]);

redis.set('key', 100, 'EX', 10);

new Redis()
new Redis(6380)
new Redis(6379, '192.168.1.1')
new Redis('/tmp/redis.sock')
new Redis({
  port: 6379,
  host: '127.0.0.1',
  family: 4,
  password: 'auth',
  db: 0
})

new Redis('redis://:authpassword@127.0.0.1:6380/4')
















```

```
```


