### ioredis
---
https://github.com/luin/ioredis

```
npm install ioredis
npm test

DEBUG=ioredis:* node app.js
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

var Redis = require('ioredis');
var Redis = new Redis();
var pub = new Redis();
redis.subscribe('news', 'music', function (err, count) {
  
  pub.publish('news', 'Hello world!');
  pub.publish('music', 'Hello again!');
});

redis.on('message', function (channel, message) {
  console.log('Receive message %s from channel %s', message, channel);
});

redis.on('messageBuffer', function (channel) {
});

redis.psubscribe('pat?ern', function (err, count) {});
redis.on('pmessage', function (pattern, channel, message) {});
redis.on('pmessageBuffer', function (pattern, channel, message) {});

redis.set('foo', Buffer.from('bar'));

redis.getBuffer('foo', function (err, result) {
});

var pipeline = redis.pipeline();
pipeline.set('foo', 'bar');
pipeline.del('cc');
pipeline.exec(function (err, results) {
});

redis.pipeline().set('foo', 'bar').del('cc').exec(function (err, results) {
});

var promise = redis.pipeline().set('foo', 'bar').get('foo').exec();
promise.then(function (result) {
});

redis.pipeline().set('foo', 'bar').get('foo', function (err, result) {
}).exec(function (err, result) {
});

redis.pipeline([
  ['set', 'foo', 'bar'],
  ['get', 'foo']
]).exec(function () {});

const length = redis.pipeline().set('foo', 'bar').get('foo').length;

redis.multi().set('foo', 'bar').get('foo').exec(function (err, results) {
});

redis.multi().set('foo').set('foo', 'new value').exec(funciton (err, results) {
});

redis.multi().set('foo', 'bar', function (err, result) {
}).exec(/* ... */);

redis.multi({ pipeline: false });
redis.set('foo', 'bar');
redis.get('foo');
redis.exec(function (err, result) {
});

redis.multi([
  ['set', 'foo', 'bar'],
  ['get', 'foo']
]).exec(function () { /* */ });

redis.pipeline().get('foo').multi().set('foo', 'bar').get('foo').exec().get('foo').exec();

var redis = new Redis();

redis.defineCommand('echo', {
  numberOfKeys: 2,
  lua: 'return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}'
});

redis.echo('k1', 'k2', 'al', 'a2', fnction (err, result) {
});

redis.echoBuffer('k1', 'k2', 'a1', 'a2', function (err, result) {
});

redis.pipeline().set('foo', 'bar').echo('k1', 'k2', 'a1', 'a2').exec();

redis.defineCommand('echoDynamicKeyNumber', {
  lua: 'return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}'
});

redis.echoDynamicKeyNumber(2, 'k1', 'k2', 'a1', 'a2', funciton (err, result) {
});

var fooRedis = new Redis({ keyPrefix: 'foo:' });
fooRedis.set('bar', 'baz');

fooRedis.defineCommand('echo', {
  numberOfKeys: 2,
  lua: 'return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}'
});

fooRedis = require('ioredis');

Redis.Command.setArgumentTransformer('hmset', function (args) {
  if (args.length === 2) {
    if(typeof Map !== 'undefined' && args[1] instanceof Map) {
      return [args[0]].concat(utils.convertMapToArray(args[1]));
    }
    if ( typeof args[1] === 'object' && args[1] !== null) {
      return [args[0]].concat(utils.convertObjectToArray(args[1]));
    }
  }
  return args;
});

Redis.Command.setReplyTransformer('hgetall', function (result) {
  if (Array.isArray(result)) {
    var obj = {};
    for(var i = 0; i < result.length; i += 2) {
      obj[result[i]] = result[i + 1];
    }
    return obj;
  }
  return result;
});

redis.mset({ k1: 'v1', k2: 'v2' });
redis.get('k1', function (err, result) {
});

redis.mset(new Map(['k3', 'v3'], ['k4', 'v4']));
redis.get('k3', function (err, result) {
});

Redis.Command.setReplyTransformer('hgetall', function (result) {
  var arr = [];
  for (var i = 0; i < result.length; i += 2) {
    arr.push([result[i], result[i + 1]]);
  }
  return arr;
});
redis.hset('h1', Buffer.from([0x01]), Buffer.from([0x02]));
redis.hset('h1', Buffer.from([0x03]), Buffer.from([0x04]));
redis.hgetallBuffer('h1', function (err, result){
});

redis.monitor(function (err, monitor) {
  monitor.on('monitor', function (time, args, source, database) {
  });
});


var redis = new Redis();
var stream = redis.scanStream();
stream.on('data', function (resultKeys) {
  for(var i = 0; i < resultKeys.length; i++){
    console.log(resultKeys[i]);
  }
});
stream.on('end', function () {
  console.log('all keys have been visited');
});

var stream = redis.scanStream({
  
  match: 'user:*',
  
  count: 100
});

var stream = redis.hscanStream('myhash', {
  match: 'age:??'
});

var stream = redis.scanStream();
stream.on('data', function (resultKeys) {
  stream.pause();
  
  Promise.all(resultKeys.map(migrateKeyToMySQL)).then(() => {
    stream.resume();
  });
});

stream.on('end', function () {
  console.log('done migration');
});

var redis = new Redis({
  retryStrategy: function (times) {
    var delay = Math.min(times * 50, 2000);
    return delay;
  }
});

var redis = new Redis({
  maxRetriesPerRequest: 1
});

var redis = new Redis({
  reconnectOnError: function (err) {
    var targetError = 'READONLY';
    if (err.message.slice(0, targetError.length) === targetError) {
      return true;
    }
  }
});


var redis = new Redis({ enableOfflineQueue: false });

var redis = new Redis({
  host: 'localhost',
  tls: {
    ca: fs.readFileSync('cert.pem')
  }
});

var redis = new Redis({
  sentinels: [{ host: 'localhost', port: 26379 }, { host: 'localhost', port: 26380 }],
  name: 'mymaster'
});

redis.set('set', 'bar');


var availableSlaves = [];

var preferredSlaves = [];

preferredSlaves = function(availableSlaves){
  for (var i = 0; i < availableSlaves.length; i++) {
    var slave = availableSlaves[i];
    if (slave.ip === '127.0.0.1') {
      if (slave.port === '31234') {
        return slave;
      }
    }
  }
  return false;
};

var redis = new Redis({
  sentinels: [{ host: '127.0.0.1', port: 26379 }, { host: '127.0.0.1', port: 26380 }],
  name: 'mymaster',
  role: 'slave',
  preferredSlaves: preferredSlaves
});

function (times) {
  var delay = Math.min(times * 10, 1000);
  return delay;
}


var Redis = require('ioredis');

var cluster = new Redis.Cluster([{
  port: 6380,
  host: '127.0.0.1'
}, {
  port: 6381,
  host: '127.0.0.1'
}]);

cluster.set('foo', 'bar');
cluster.get('foo', function (err, res) {
});

function (times) {
  var delay = Math.min(100 + times * 2, 2000);
  return delay;
}

function (times) {
  this.startNodes = [{ port: 6790, host: '127.0.0.1' }];
  return Math.min(100 + times * 2, 2000);
}

var cluster = new Redis.Cluster([/**/], {
  scaleReads: 'slave'
});
cluster.set('foo', 'bar');
cluster.get('foo', function (err, res) {
});

var slaves = cluster.nodes('slaves');
Promise.all(slaves.map(function (node) {
  return node.flushdb();
}));

var masters = cluser.nodes('master');
Promise.all(master.map(function (node) {
  return node.keys();
})).then(function (keys) {
});


const cluster = new Redis.Cluster([{
  host: '203.0.113.73',
  port: 30001
}], {
  natMap: {
    '10.0.1.230:30001': {host: '203.0.113.73', port: 30001},
    '10.0.1.231:30001': {host: '203.0.113.73', port: 30002},
    '10.0.1.232:30001': {host: '203.0.113.73', port: 30003}
  }
})


var nodes = [/**/];
var pub = new Redis.Cluster(nodes);
var sub = new Redis.Cluster(nodes);
sub.on('message', function (channel, message) {
  console.log(channel, message);
});

sub.subscribe('news', function () {
  pub.publish('news', 'highlights');
});

var Redis = require('ioredis');
var cluster = new Redis.Cluster(nodes, {
  redisOptions: {
    password: 'your-cluster-password'
  }
});

var Redis = require('ioredis');
var cluster = new Redis.Cluster([
  { port: 30001, password: 'password-for-30001'},
  { port: 30002, password: null }
], {
  redisOptions: {
    password: 'fallback-password'
  }
});

var Redis = require('ioredis');
var redis = new Redis();

redis.set('foo', function (err) {
  err instanceof Redis.ReplyError
});

var Redis = require('ioredis');
var redis = new Redis({ showFriendlyErrorStack: true });
redis.set('foo');

const Redis = require('ioredis')
Redis.Promise = require('bluebird')

const redis = new Redis()

assert.equal(redis.gt().constructor, require('bluebird'))

Redis.Promise = global.Promise
assert.equal(redis.get().constructor, global.Promise)

```

```

```


