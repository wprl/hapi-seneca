seneca-hapi
===========

A seneca plugin for working with the Hapi.js framework.

Usage
-----

This module returns a Hapi plugin that allows [seneca-web](https://github.com/rjrodger/seneca-web) functionality to work as expected in the Hapi framework.

**params** - When used as a plugin in the Hapi framework, the `options` object should have the `seneca` option referencing the seneca instance that any seneca-web actions have been called on.

The `cors` option should be set to true if the API is to be accessed by other web hosts.

**returns** - a Hapi plugin that can be registered to a Hapi instance.

Example
-------

```JavaScript
var Hapi = require('hapi');
var seneca = require('seneca')();

var server = new Hapi.Server({ port: 4000 });

seneca.add({role: 'test', cmd: 'test'}, function(args, done) {
  console.log('Recieved a request');
  done(null, {worked: true});
});

seneca.act('role:web', {use: {
  prefix: '/api/1.0',
  pin: {role:'test', cmd: '*'},
  map: {
    test: true
  }
}});

server.register({
  register: require('hapi-seneca'),
  options: {
    seneca: seneca,
    cors: true
  }
}, function(err) {
  if (err) console.error(err);
  server.start(function() {
    console.log('Server is running');
  });
});
```

Note
----

Most of the logic for this module lies in the [hapi-to-express](https://github.com/jrpruit1/hapi-to-express) module.
