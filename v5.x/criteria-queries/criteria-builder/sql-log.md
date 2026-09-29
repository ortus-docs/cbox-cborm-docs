---
description: "See the HQL and SQL a criteria produces with getSQL(), peekSQL(), logSQL() and the SQL log"
---

# SQL Log & Debugging

The criteria builder can show you the HQL it compiles to and the SQL Hibernate will run, without running the query. This is the fastest way to understand why a criteria returns what it returns.

## Seeing the query

| Method (aliases) | Description |
| ---------------- | ----------- |
| `getSQL( [returnExecutableSql=false], [formatSql=true] )` (`toSQL`) | The SQL the criteria runs for `list()`, without running it. `returnExecutableSql = true` puts the bound values in the SQL, ready to paste into a SQL tool; `formatSql = false` keeps it on one line. Paging is not included. The arguments are also accepted as `executable` and `format` |
| `getHQL()` (`toHQL`) | The HQL the criteria compiles to |
| `peekSQL( callback, [returnExecutableSql=false] )` | Calls the closure with the SQL and keeps chaining |
| `peek( callback )` (`tap`) | Calls the closure with the criteria and keeps chaining |
| `toString()` / `writeDump( c )` | The calls you made, the HQL, the parameters and the SQL |

```javascript
var c = userService
    .newCriteria()
    .isEq( "userName", "joe" )
    .like( "firstName", "%joe%" )
    .isEq( "role.slug", "admin" );

// SQL with ? placeholders
writeOutput( c.getSQL() );

// SQL with the values in place
writeOutput( c.getSQL( returnExecutableSql = true ) );

// The HQL
writeOutput( c.getHQL() );

// Everything at once
writeDump( c );

// Inside a chain
var users = userService
    .newCriteria()
    .isTrue( "isActive" )
    .peekSQL( ( sql ) => log.debug( sql ) )
    .list();
```

## The SQL log

The SQL log is a list of SQL snapshots kept on the criteria. Each `logSQL()` call appends one entry with the SQL as it is at that point of the build, and also writes it to the ORM log.

| Method (aliases) | Description |
| ---------------- | ----------- |
| `logSQL( [label="Criteria"], [returnExecutableSql], [formatSql] )` | Appends `{ type : label, sql : "..." }` to the SQL log and writes the SQL to the ORM log |
| `getSqlLog()` | The SQL log entries, oldest first |
| `startSqlLog( [returnExecutableSql=false], [formatSql=false] )` | Turns the SQL log on and sets how later `logSQL()` calls write the SQL |
| `stopSqlLog()` | Turns the SQL log off. Entries already logged are kept |
| `canLogSql()` (`getSqlLoggerActive`) | Whether `startSqlLog()` turned the log on |

```javascript
var c = userService
    .newCriteria()
    .startSqlLog( returnExecutableSql = true );

c.isEq( "firstName", "Luis" ).logSQL( "by first name" );
c.like( "lastName", "M%" ).logSQL( "by last name" );

writeDump( c.getSqlLog() );
// [ { type : "by first name", sql : "select ..." }, { type : "by last name", sql : "select ..." } ]

c.stopSqlLog();
c.canLogSql(); // false
```

Without `startSqlLog()`, `logSQL()` writes executable, formatted SQL by default. `copy()` copies the SQL log along with the criteria.

{% hint style="warning" %}
`getSQL()` and the SQL log rely on bx-orm's Hibernate statement inspector. If your ORM settings configure your own (`hibernate.session_factory.statement_inspector`), `getSQL()` raises an error that says so.
{% endhint %}

{% hint style="info" %}
**Changes from cborm 5**

* Nothing is logged automatically: `startSqlLog()` no longer records the SQL at every step of the build. Call `logSQL()` wherever you want an entry.
* `getPositionalSQLParameters()` and the other `SQLHelper` methods no longer exist: use `getSQL( returnExecutableSql = true )`, `peekSQL()`, `logSQL()` or `writeDump( c )`, which shows the parameters.
{% endhint %}
