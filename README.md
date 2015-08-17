# Cordova SQLite Plugin

This fork of [Cordova-sqlite-storage](https://github.com/litehelpers/Cordova-sqlite-storage) for Cordova version 4 and above.

# Usage

The API is the same as [Web SQL API](http://www.w3.org/TR/webdatabase/)

```js
document.addEventListener("deviceready", function() {
  window.openDatabase = window.sqlitePlugin.openDatabase;
  // now can be used instead of WebSQL API
}, false);
```

**NOTE:** If a sqlite statement in a transaction fails with an error, the error handler *must* return `false` in order to recover the transaction. This is correct according to the HTML5/[Web SQL API](http://www.w3.org/TR/webdatabase/) standard. This is different from the WebKit implementation of Web SQL in Android and iOS which recovers the transaction if a sql error hander returns a non-`true` value.

## Opening a database

There are two options to open a database:
- **Recommended:** `var db = window.sqlitePlugin.openDatabase({name: "my.db", location: 1});`
- **Classical:** `var db = window.sqlitePlugin.openDatabase("myDatabase.db", "1.0", "Demo", -1);`

The new `location` option is used to select the database subdirectory location (iOS *only*) with the following choices:
- `0` (default): `Documents` - visible to iTunes and backed up by iCloud
- `1`: `Library` - backed up by iCloud, *NOT* visible to iTunes
- `2`: `Library/LocalDatabase` - *NOT* visible to iTunes and *NOT* backed up by iCloud

### Pre-populated database

For Android, iOS, and Amazon Fire-OS (*only*): put the database file in the `www` directory and open the database like:

```js
var db = window.sqlitePlugin.openDatabase({name: "my.db", createFromLocation: 1});
```

or to disable iCloud backup:

```js
db = sqlitePlugin.openDatabase({name: "my.db", location: 2, createFromLocation: 1});
```

**IMPORTANT NOTES:**

- Put the pre-populated database file in the `www` subdirectory. This should work well with using the Cordova CLI to support both Android & iOS versions.
- The pre-populated database file name must match **exactly** the file name given in `openDatabase`. The automatic extension has been completely eliminated.
- The pre-populated database file is ignored if the database file with the same name already exists in your database file location.

**TIP:** If you don't see the data from the pre-populated database file, completely remove your app and try it again!
