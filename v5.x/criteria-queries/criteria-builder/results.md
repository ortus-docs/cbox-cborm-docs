---
description: "Terminal methods that run a criteria query, and the result shapes they return"
---

# Results

A criteria only describes a query. It runs when you call a **terminal method**, which returns the results. Terminal methods never change the criteria, so you can call several of them on the same criteria (`count()` and then `list()`, in any order).

| Method                                              | Returns |
| --------------------------------------------------- | ------- |
| `list( [max], [offset], [timeout], [sortOrder], [ignoreCase], [asQuery] )` | The matching rows |
| `count( [property] )`                               | The number of matching entities, or of distinct values of a property |
| `get( [uniqueFirst=false] )`                        | The single match, or `null`. More than one match is an `orm.query.nonUnique` error unless `uniqueFirst` is true |
| `getOrFail( [uniqueFirst=false] )`                  | Like `get()`, but no match is an `orm.notFound` error |
| `first()`                                           | The first row in order, or `null` |
| `firstOrFail()`                                     | Like `first()`, but no row is an `orm.notFound` error |
| `exists()`                                          | Whether any row matches |
| `paginate( [page=1], [maxRows=25] )`                | `{ results, pagination : { page, maxRows, totalRecords, totalPages } }` |
| `simplePaginate( [page=1], [maxRows=25] )`          | `{ results, pagination : { page, maxRows, hasMore } }`, without a count query |
| `pluck( property )`                                 | One property's values, in order |
| `sum( property )`, `avg( property )`, `min( property )`, `max( property )` | An aggregate value |
| `each( callback, [size=100] )`                      | Calls the closure once per row, reading in batches; returns the row count |
| `chunk( size, callback )`                           | Calls the closure with each batch of rows; returns the row count |
| `updateAll( values )`                               | Updates every matching row with one `UPDATE`; returns the row count |
| `deleteAll()`                                       | Deletes every matching row with one `DELETE`; returns the row count |

## `list()`

`list()` returns the matching rows: an array of entities by default, or the shape chosen with the [result modifiers](#result-shapes). Its optional arguments are applied to a copy of the criteria, so they don't change it:

| Argument     | Description |
| ------------ | ----------- |
| `max`        | The maximum number of rows (0 means all) |
| `offset`     | The number of rows to skip |
| `timeout`    | The query timeout, in seconds |
| `sortOrder`  | The ordering, for example `"lastName asc, firstName desc"` |
| `ignoreCase` | Sort text properties case-insensitively |
| `asQuery`    | Return a BoxLang query |

```javascript
var c = commentService
    .newCriteria()
    .isTrue( "isApproved" )
    .when( len( arguments.postId ), ( c ) => c.isEq( "post.id", postId ) )
    .when( len( arguments.siteId ), ( c ) => c.isEq( "post.site.id", siteId ) );

var results = {
    count    : c.count(),
    comments : c.list(
        offset    = arguments.offset,
        max       = arguments.max,
        sortOrder = "createdDate #arguments.sortOrder#"
    )
};
```

{% hint style="info" %}
Use named arguments with `list()`: its positional order is `max, offset, timeout, sortOrder, ignoreCase, asQuery`.
{% endhint %}

## `count( [property] )`

Without an argument, `count()` returns the number of matching root entities (each entity is counted once, even when a condition goes through a to-many association). With a property, it counts the distinct values of that property.

```javascript
var total = postService
    .newCriteria()
    .isEq( "categories.slug", "boxlang" )
    .isTrue( "isPublished" )
    .isLe( "publishedDate", now() )
    .or(
        postService.getRestrictions().isNull( "expireDate" ),
        postService.getRestrictions().isGt( "expireDate", now() )
    )
    .cache( true )
    .count();

var authors = postService.newCriteria().isTrue( "isPublished" ).count( "author.id" );
```

## `get()` and `getOrFail()`

`get()` returns the single entity that matches, or `null` when none does. `getOrFail()` raises an `orm.notFound` error instead of returning `null`. Both raise an `orm.query.nonUnique` error when more than one row matches, unless you pass `uniqueFirst = true` to take the first one.

```javascript
var category = categoryService
    .newCriteria()
    .isEq( "slug", arguments.slug )
    .isEq( "site.slug", arguments.siteSlug )
    .get();

var user = userService.newCriteria().isEq( "email", rc.email ).getOrFail();
```

### Getting a struct of properties

In cborm 5, `get( properties )` and `getOrFail( properties )` returned a struct of the given properties. In cborm 6, select the properties with `withProjections()` and ask for a struct with `asStruct()`:

```javascript
// cborm 5
var data = newCriteria().isEq( "slug", slug ).get( "id,slug" );

// cborm 6
var data = newCriteria()
    .isEq( "slug", slug )
    .withProjections( property = "id,slug" )
    .asStruct()
    .get();
```

## `first()`, `exists()` and `pluck()`

```javascript
// The latest post, or null
var latest = postService.newCriteria().order( "publishedDate", "desc" ).first();

// Is the email taken?
var taken = userService.newCriteria().isEq( "email", rc.email ).exists();

// The emails of all active users, in order
var emails = userService.newCriteria().isTrue( "isActive" ).order( "email" ).pluck( "email" );
```

## Pagination

`paginate()` runs a count and a page query. `simplePaginate()` skips the count and only tells you whether there is a next page.

```javascript
var page = userService
    .newCriteria()
    .isTrue( "isActive" )
    .order( "lastName" )
    .paginate( page = rc.page ?: 1, maxRows = 25 );

// page.results, page.pagination.totalRecords, page.pagination.totalPages
```

## Aggregates

```javascript
var c = orderService.newCriteria().isEq( "status", "paid" );

var revenue = c.sum( "total" );
var average = c.avg( "total" );
var largest = c.max( "total" );
var oldest  = c.min( "createdDate" );
```

For grouped aggregates, see [Projections & Aggregates](projections.md).

## Batches: `each()` and `chunk()`

`each()` and `chunk()` read the rows in batches (100 by default for `each()`). After each batch the session is flushed, so changes made in the callback are saved, and cleared, so memory stays flat.

```javascript
userService
    .newCriteria()
    .isFalse( "isVerified" )
    .chunk( 500, ( users ) => {
        users.each( ( u ) => u.setReminderSent( true ) );
    } );
```

## Bulk updates and deletes

`updateAll( values )` and `deleteAll()` change every matching row with a single statement, without loading any entity. Both return the number of rows changed.

```javascript
// One UPDATE
var expired = orderService
    .newCriteria()
    .isEq( "status", "pending" )
    .isLt( "createdDate", dateAdd( "d", -30, now() ) )
    .updateAll( { status : "expired" } );

// One DELETE
var removed = sessionService.newCriteria().isLt( "expires", now() ).deleteAll();
```

{% hint style="warning" %}
Bulk statements run in the database only: no entity events fire, nothing cascades, versions and timestamps are not updated, and entities already loaded in the session keep their old values. They cannot be combined with `maxResults()` or `firstResult()`.
{% endhint %}

## Result shapes

| Modifier                              | What `list()` returns |
| ------------------------------------- | --------------------- |
| (default)                             | An array of entities |
| `asStruct()`                          | An array of structs. Without projections: the id and plain properties of each entity (dates as ISO 8601 strings). With projections: one key per projection alias |
| `asStruct( includes, [options] )`     | Structs built like bx-orm's `entityToStruct()`, read with projection queries instead of loading entities |
| `asQuery()`                           | A BoxLang query |
| `asStream()`                          | A Java stream |
| `asDistinct()`                        | Distinct rows |

With [projections](projections.md) and no `asStruct()`/`asQuery()`, one projection returns plain values and several return one array per row.

```javascript
// Array of structs
var users = userService
    .newCriteria()
    .isTrue( "isActive" )
    .withProjections( property = "id,firstName,lastName" )
    .asStruct()
    .list();

// Query
var users = userService.newCriteria().isTrue( "isActive" ).asQuery().list();
var users = userService.newCriteria().isTrue( "isActive" ).list( asQuery = true );

// Structs with associations, without loading entities
var users = userService
    .newCriteria()
    .isTrue( "isActive" )
    .order( "lastName" )
    .asStruct( "id,lastName,role.name,posts" )
    .paginate( page = 1, maxRows = 25 );
```

`asStruct( includes )` works with `list()`, `get()`, `first()` and `paginate()`, not with `each()` or `chunk()`.

{% hint style="info" %}
**Changes from cborm 5**

* `get( properties )` and `getOrFail( properties )` became `withProjections( property = "..." ).asStruct().get()`: the argument of `get()` and `getOrFail()` is now `uniqueFirst`.
* `list( asStream = true )` is replaced by `asStream().list()`, which returns a Java stream. To keep using cbStreams, wrap the array from `list()` with `StreamBuilder@cbStreams`.
* `count()` and `list()` can be called on the same criteria in any order.
* `getOrFail()` raises `orm.notFound` (cborm 5: `EntityNotFound`), and more than one match raises `orm.query.nonUnique`.
* `first()`, `firstOrFail()`, `exists()`, `paginate()`, `simplePaginate()`, `pluck()`, the aggregate terminals, `each()`, `chunk()`, `updateAll()` and `deleteAll()` are new.
{% endhint %}
