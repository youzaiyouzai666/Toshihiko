# Toshihiko

A simple ORM for node.js in Huaban.

## Installation

```sh
$ npm install toshihiko
```

## Document

### Initialize

You should create a `Toshihiko` object to connect to MySQL:

```javascript
var T = require("toshihiko");
var toshihiko = new T.Toshihiko(database, username, password, options);
```

***Options*** can include these things:

+ `host`: hostname or IP of MySQL. Defaults to `localhost`.
+ `port`: port of MySQL. Defaults to `3306`.
+ `memcached`: if you want to memcached support, let it be an `Memcached` object which will be mentioned below. Defaults
  to undefined.
+ etc... (All options in module [mysql](https://www.npmjs.org/package/mysql#pool-options) will be OK)

#### Memcached

If you want to have memcached supported, you should create a `Memcached` object and then put it into options:

```javascript
var Memcached = T.Memcached;
var toshihiko = new T.Toshihiko(database, username, password, {
    memcached   : new Memcached(servers, options);
});
```

> **Notice:** the `servers` and `options` parameters can be referenced at https://www.npmjs.org/package/memcached#setting-up-the-client.

### Define a Model

Define a model schema:

```javascript
var Model = toshihiko.define(tableName, [
    { name: "key1", alias: "keyOne", primaryKey: true, type: Toshihiko.Type.Integer },
    { name: "key2", type: Toshihiko.Type.String, defaultValue: "Ha~" },
    { name: "key3", type: Toshihiko.Type.Json, defaultValue: [] },
    { name: "key4", validators: [
        function(v) {
            if(v > 100) return "`key4` can't be greater than 100";
        },
        function(v) {
            // blahblah...
        }
    ] }
]);
```

You can add extra model functions by yourself:

```javascript
Model.sayHello = function() {
    this.find(function(err, rows) {
        console.log(err);
        console.log(rows);
    });
};
```