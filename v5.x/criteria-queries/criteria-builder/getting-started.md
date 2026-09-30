---
description: "Get a criteria builder with newCriteria() and run your first criteria queries"
---

# Getting Started

You get a criteria builder from a Base ORM service, a Virtual Entity Service or an Active Entity by calling `newCriteria()`. The criteria is bound to one entity, the root of the query. It is a bx-orm criteria builder, the same object `entityCriteria( entityName )` returns.

## `newCriteria()`

| Argument           | Type    | Required | Default                  | Description                                                                                  |
| ------------------ | ------- | -------- | ------------------------ | -------------------------------------------------------------------------------------------- |
| `entityName`       | string  | true     | ---                      | The entity to query, the root of the criteria. Not passed on virtual services or Active Entities. |
| `useQueryCaching`  | boolean | false    | `false`                  | Cache the query results in the second-level query cache (same as calling `cache( true, region )`). |
| `queryCacheRegion` | string  | false    | `criterias.{entityName}` | The query cache region used when `useQueryCaching` is true.                                  |
| `datasource`       | string  | false    | The service datasource   | Ignored: the criteria always runs on the entity's own datasource.                            |

{% hint style="warning" %}
If you call `newCriteria()` from a virtual service or an Active Entity, don't pass the `entityName` argument: it roots itself automatically.
{% endhint %}

```javascript
// Base ORM service
ormService.newCriteria( "User" );

// Virtual entity service
userService.newCriteria();

// Active Entity
getInstance( "User" ).newCriteria();

// With query caching
userService.newCriteria( useQueryCaching = true, queryCacheRegion = "users.active" );
```

## Restrictions

Restrictions are your `WHERE` conditions. Each one is added with `and`. The builder offers every cborm restriction (`isEq`, `like`, `between`, `isIn`, `isNull`, `sizeGt`, ...), groups with `or()`, `and()` and `not()`, a `not` prefix for any condition, and native SQL through `sql()`. See [Restrictions](restrictions/README.md).

```javascript
// Posts that cost more than 30
postService
    .newCriteria()
    .isGe( "price", 30 )
    .count();

// The most recent active user who has logged in
userService
    .newCriteria()
    .isTrue( "isActive" )
    .isNotNull( "lastLogin" )
    .order( "lastLogin", "desc" )
    .first();

// cborm style groups with c.restrictions
var c = userService.newCriteria();
var users = c
    .like( "firstName", "Lui%" )
    .and(
        c.restrictions.between( "balance", 200, 300 ),
        c.restrictions.isEq( "department", "development" )
    )
    .maxResults( 50 )
    .order( "balance", "desc" )
    .list();

// Closure style groups
var users = userService
    .newCriteria()
    .or( ( c ) => c.isEq( "role.name", "admin" ).isGt( "age", 30 ) )
    .list();
```

{% hint style="success" %}
**Tip**: Every condition can be negated with the `not` prefix: `notEq()`, `notIn()`, `notLike()`, `notBetween()`, `notIsNull()`, ...
{% endhint %}

## Associations

Use a dotted path (`role.name`) and the builder joins the association for you, or join it yourself with `joinTo()`, `leftJoin()` and `with{Association}()`. See [Associations](associations.md).

## Modifiers

Modifiers change how the query runs: ordering, paging, caching, timeouts, read-only entities, locking and more. See [Modifiers](modifiers.md).

```javascript
var users = userService
    .newCriteria()
    .like( "firstName", "Lui%" )
    .order( "balance", "desc" )
    .firstResult( 25 )
    .maxResults( 50 )
    .timeout( 5 )
    .list();
```

## Results

A criteria is only a description of a query: nothing runs until you call a terminal method. The most common ones are:

* `list()`: the matching rows (entities by default).
* `get()`: the single match, or `null`.
* `getOrFail()`: the single match, or an `orm.notFound` error.
* `count()`: the number of matching entities.
* `paginate( page, maxRows )`: one page of results plus the pagination data.

The result shape can be changed with `asStruct()`, `asQuery()` or `asStream()`, and `withProjections()` selects columns instead of entities. See [Results](results.md) and [Projections & Aggregates](projections.md).

```javascript
var c = postService.newCriteria().isTrue( "isPublished" );

var total = c.count();
var posts = c.list( max = 10, offset = 0, sortOrder = "publishedDate desc" );
```

## Logging

You can see the HQL and SQL a criteria produces without running it (`getSQL()`, `getHQL()`, `writeDump( c )`), and keep a log of SQL snapshots with `startSqlLog()`, `logSQL()` and `getSqlLog()`. See [SQL Log & Debugging](sql-log.md).

```javascript
var sql = userService
    .newCriteria()
    .isEq( "userName", "joe" )
    .like( "firstName", "%joe%" )
    .getSQL( executable = true );
```

## Best Practices

### One criteria per query

A criteria builder is mutable: every building method changes it. Create a new one with `newCriteria()` for each query and never keep one in a shared scope (application, singleton service properties), where concurrent requests would add conditions to the same object. When you need several variations of the same base query, branch with `copy()`.

```javascript
// Good: a new criteria per call
function getUsersByStatus( required string status ){
    return newCriteria()
        .isEq( "status", arguments.status )
        .list();
}

// Bad: a criteria shared between calls and requests
variables.userCriteria = newCriteria(); // Don't do this!

// Branching a base query
var base   = newCriteria().isTrue( "isActive" );
var admins = base.copy().isEq( "role.name", "admin" ).list();
var total  = base.count();
```

### Performance

* **Cache stable queries**: `useQueryCaching` or `cache( true, region )` store results in the second-level query cache.
* **Select only what you need**: projections and `asStruct()` avoid loading full entity graphs.
* **Page large results**: use `paginate()`, `maxResults()` or `list( max, offset )`, and `each()` or `chunk()` to walk very large result sets in batches.
* **Watch the SQL**: use `getSQL()` and `logSQL()` during development.

```javascript
postService
    .newCriteria( useQueryCaching = true, queryCacheRegion = "posts.published" )
    .isTrue( "isPublished" )
    .withProjections( property = "id,title,slug" )
    .asStruct()
    .list();
```
