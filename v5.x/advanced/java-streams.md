---
description: Stream results from the database with native Java streams
---

# Java Streams

cborm 6 returns native Java streams, built by `bx-orm`: no extra module is needed (cbStreams is no longer a dependency). Pass `asStream = true` to `list()`, `executeQuery()`, `findAll()`, `findAllWhere()`, `getAll()` or a dynamic finder, or call `asStream()` on a criteria query.

A stream reads the rows from the database as you consume it, so a large result never sits in memory at once. BoxLang closures work with every stream method:

```javascript
// names of the active users, read row by row
var names = userService.list( asStream = true )
    .filter( ( user ) => user.getIsActive() )
    .map( ( user ) => user.getFullName() )
    .toList();

// count without building an array
var total = ormService.findAllWhere( entityName = "Order", criteria = { status : "open" }, asStream = true ).count();

// criteria
var emails = ormService.newCriteria( "User" )
    .isTrue( "isActive" )
    .asStream()
    .list()
    .map( ( user ) => user.getEmail() )
    .toList();
```

{% hint style="warning" %}
A stream holds an open database cursor until it is exhausted or closed, so consume it in the same request. Hibernate closes it with the session at the latest.
{% endhint %}

## Rules

* A unique query (`unique = true`) or an update/delete cannot be streamed: `bx-orm` raises an `orm.argument` error. A `findBy...` dynamic finder with `asStream` returns a stream of zero or one entity.
* `asQuery` does not apply to streams.

## Coming from cbStreams

cborm 5 returned [cbStreams](https://forgebox.io/view/cbstreams) streams. The native stream has the same core methods (`filter`, `map`, `reduce`, `forEach`, `count`, `anyMatch`, `sorted`, `limit`, `skip`, ...). If you rely on a cbStreams-only method, install cbStreams yourself and build one from the array result: `streamBuilder.new( ormService.list( "User" ) )`.
