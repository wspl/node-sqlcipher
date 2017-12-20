Fork of [node-sqlite3](https://github.com/mapbox/node-sqlite3), modified to use [SQLCipher](https://www.zetetic.net/sqlcipher/).

While the `node-sqlite3` project does include support for compiling against sqlcipher, it requires manual work, and does not work out-of-the-box on Electron on Windows. This fork changes the default configuration to bundle SQLCipher directly, as well as OpenSSL where required.

## Supported platforms

Tested on Node v8.x, and Electron 1.7.9.

# Installing

```sh
yarn install "@journeyapps/sqlcipher"
# Or: npm install --save "@journeyapps/sqlcipher"
```

For electron, this should work with `electron-rebuild`.

# Usage

``` js
var sqlite3 = require('@journeyapps/sqlcipher').verbose();
var db = new sqlite3.Database('test.db');

db.serialize(function() {
  db.run("PRAGMA key = 'mysecret'");
  db.run("CREATE TABLE lorem (info TEXT)");

  var stmt = db.prepare("INSERT INTO lorem VALUES (?)");
  for (var i = 0; i < 10; i++) {
      stmt.run("Ipsum " + i);
  }
  stmt.finalize();

  db.each("SELECT rowid AS id, info FROM lorem", function(err, row) {
      console.log(row.id + ": " + row.info);
  });
});

db.close();
```

# SQLCipher

A copy of the source for SQLCipher 3.4.1 is bundled, based on SQLite 3.15.2.

On Windows, OpenSSL 1.0.2n is additionally bundled. This is specifically used for Electron builds. Pre-built libraries are used from https://slproweb.com/products/Win32OpenSSL.html.

On other platforms, OpenSSL is provided by NodeJS.

# API

See the [API documentation](https://github.com/mapbox/node-sqlite3/wiki) in the wiki.

# Testing

[mocha](https://github.com/visionmedia/mocha) is required to run unit tests.

In sqlite3's directory (where its `package.json` resides) run the following:

    npm install mocha
    npm test

# Acknowledgments

Most of the work in this library is from the [node-sqlite3](https://github.com/mapbox/node-sqlite3) library by [MapBox](http://mapbox.org/).

Additionally, some of the SQLCipher-related changes are based on a fork by [liubiggun](https://github.com/liubiggun/node-sqlite3).

# License

`node-sqlcipher` is [BSD licensed](https://github.com/mapbox/node-sqlite3/raw/master/LICENSE).
