# connect-memcached

  Memcached session store, using [node-memcached](http://github.com/3rd-Eden/node-memcached) for communication with cache server.

## Installation

  via npm:

      $ npm install connect-memcached

## Example
```javascript
      /**
      * Module dependencies.
      */

      var express = require('express');

      // pass the express to the connect memcached module
      // allowing it to inherit from express.session.Store
      var MemcachedStore = require('connect-memcached')(express);

      var app = express();

      app.use(express.favicon());

      // request logging
      app.use(express.logger());

      // required to parse the session cookie
      app.use(express.cookieParser());

      // Populates:
      // - req.session
      // - req.sessionStore
      // - req.sessionID (or req.session.id)

      app.use(express.session({
        secret: 'CatOnTheKeyboard',
        store: new MemcachedStore({
          hosts: [ '127.0.0.1:11211' ] // Change this to your memcache server(s). See Options for additional info.
        })
      }));

      app.get('/', function(req, res){
        if (req.session.views) {
          ++req.session.views;
        } else {
          req.session.views = 1;
        }
        res.send('Viewed <strong>' + req.session.views + '</strong> times.');
      });

      app.listen(3000);
      console.log('Express app started on port 3000');
```

## Options
- `hosts` Memcached servers locations, can by string, array, hash.
- `prefix` An optional prefix for each memcache key, in case you are sharing
           your memcached servers with something generating its own keys.
- ...     Rest of given option will be passed directly to the node-memcached constructor.

For details see [node-memcached](http://github.com/3rd-Eden/node-memcached).

## Contributors

Big thanks for the priceless contributors! See the actual list [here](https://github.com/balor/connect-memcached/graphs/contributors)!

## License

[MIT](LICENSE) © 2017 Shutterstock Images, LLC
