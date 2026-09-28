---
description: "Ordering, paging, caching, timeouts, read-only, locking and result shape modifiers"
---

# Modifiers

Modifiers change how a criteria query runs or what shape its results take. Like conditions, they return the criteria, so they chain. They can be called in any order before the terminal method.

## Query modifiers

| Method (aliases)                                                   | Description |
| ------------------------------------------------------------------ | ----------- |
| `order( property, [direction="asc"], [ignoreCase=false] )` (`orderBy`, `sort`) | Add an ordering. `property` can hold a direction and several orderings: `"lastName desc, firstName"`. `ignoreCase` sorts text properties case-insensitively. Call it as often as you like |
| `orderByDesc( property )` (`sortDesc`)                             | Descending ordering |
| `firstResult( offset )` (`offset`, `skip`)                         | Skip this many rows (0-based) |
| `maxResults( max )` (`limit`, `take`)                              | Return at most this many rows |
| `cache( [cache=true], [region] )` (`cacheable`)                    | Cache the results in the second-level query cache, optionally in a region. `cache( "region" )` also works, and `cache( false )` turns caching off. Needs the ORM second-level cache to be enabled |
| `readOnly( [readOnly=true] )`                                      | Loaded entities are read-only: their changes are not saved on flush |
| `timeout( seconds )`                                               | JDBC query timeout, in **seconds** |
| `fetchSize( size )`                                                | JDBC fetch size |
| `comment( text )`                                                  | Add a comment to the generated SQL (visible when SQL logging is on) |
| `queryHint( name, value )` (`hint`)                                | Set a Hibernate query hint, for example `queryHint( "org.hibernate.readOnly", true )` |
| `lock( [mode="write"], [options] )`                                | Lock the returned rows until the transaction ends, see [Locking rows](#locking-rows) |

```javascript
c.timeout( 5 );
c.readOnly();
c.firstResult( 20 ).maxResults( 50 ).fetchSize( 10 ).cache( true, "my.awesome.region" );
c.order( "lastName", "desc", true );
c.orderBy( "lastName desc, firstName asc" );
c.comment( "Dashboard: active users" );
```

Without an explicit `order()`, entity results follow the entity's `defaultSort` when it declares one.

### Locking rows

`lock( mode, options )` locks the rows that `list()`, `get()` and `first()` return, until the transaction ends. Counts and aggregates are not locked. The mode is `write` (the default, exclusive), `read` (shared) or `force` (exclusive, and increments the version). Options are `timeout` (seconds to wait, `0` means do not wait) and `skipLocked`. `lock()` must run inside a `transaction{}` block, otherwise an `orm.argument` error is raised.

```javascript
// A queue worker that claims up to 10 new jobs, skipping jobs another worker has locked
transaction {
    var jobs = jobService
        .newCriteria()
        .isEq( "status", "new" )
        .lock( "write", { skipLocked : true } )
        .maxResults( 10 )
        .list();
    jobs.each( ( job ) => job.setStatus( "running" ) );
}
```

## Flow helpers

| Method (aliases)                              | Description |
| --------------------------------------------- | ----------- |
| `when( test, callback, [otherwise] )`         | Call `callback` with the criteria when `test` is true, else `otherwise` if given. `test` can be a boolean or a closure that receives the criteria |
| `unless( test, callback, [otherwise] )`       | The opposite of `when()` |
| `apply( callback )` (`scope`)                 | Call a reusable closure with the criteria |
| `peek( callback )` (`tap`)                    | Call a closure with the criteria and keep chaining, for example to log it |
| `copy()` (`clone`)                            | An independent copy of the criteria to branch from |

```javascript
userService
    .newCriteria()
    .isEq( "isActive", true )
    .peek( ( c ) => log.debug( "Current SQL: #c.getSQL()#" ) )
    .when( !isNull( arguments.role ), ( c ) => c.isEq( "role.name", role ) )
    .list();
```

## Result modifiers

These choose the shape of what the terminal methods return. See [Results](results.md) for details.

| Method (aliases)                   | Result |
| ---------------------------------- | ------ |
| `asEntities()` (`asArray`)         | Entities (the default) |
| `asStruct( [includes], [options] )` (`asStructs`) | One struct per row |
| `asQuery()`                        | A BoxLang query |
| `asStream()`                       | A Java stream |
| `asDistinct()` (`distinct`)        | Distinct rows |

{% hint style="info" %}
**Changes from cborm 5**

* `cacheRegion( name )` is replaced by `cache( true, name )` (or `cache( name )`).
* `timeout()` is in seconds, as in JDBC.
* `queryHint( name, value )` sets a named Hibernate/JPA query hint (`query.setHint()`), instead of adding a database-specific hint to the SQL.
* `asDistinct()` makes the rows distinct in the query (`select distinct`) instead of applying Hibernate's `DISTINCT_ROOT_ENTITY` result transformer. Entity lists that join a to-many association are made distinct automatically.
* `asStruct()` no longer uses Hibernate's `ALIAS_TO_ENTITY_MAP` transformer, and `asStream()` returns a Java stream instead of a cbStreams stream.
* `resultTransformer()` no longer exists.
{% endhint %}
