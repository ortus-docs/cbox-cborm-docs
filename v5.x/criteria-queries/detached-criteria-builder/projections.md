---
description: "Choose what a subquery selects with withProjections() or project()"
---

# Projections

A subquery used in a comparison (`propertyIn()`, `subEq()`, `isIn()`, ...) must select one value per row: a property, an id or an aggregate. You choose it with a projection, exactly as on a main criteria: `withProjections()` or `project()`. See [Projections & Aggregates](../criteria-builder/projections.md).

```javascript
var c = userService.newCriteria();

// The subquery selects the post's author id
var authors = c
    .propertyIn(
        "id",
        c.subquery( "Post", "p" )
            .isEq( "status", "draft" )
            .withProjections( property = "author.id" )
    )
    .list();

// The same with project()
var c = userService.newCriteria();
var authors = c
    .propertyIn(
        "id",
        c.subquery( "Post", "p" )
            .isEq( "status", "draft" )
            .project( ( p ) => p.property( "author.id" ) )
    )
    .list();
```

## What a subquery selects

* The **first** projection is the value the subquery selects. Declare one projection per subquery.
* `groupProperty` projections add a `group by`, for example to compare with a per-group aggregate.
* `distinct` (or `asDistinct()`) makes the subquery `select distinct`.
* Without a projection, the subquery selects its entity, which is all `exists()` and `notExists()` need. For comparisons, always declare a projection.

```javascript
// Aggregates: users whose balance is above the average
var c = userService.newCriteria();
var aboveAverage = c
    .propertyGt( "balance", c.subquery( "User", "u2" ).withProjections( avg = "balance" ) )
    .list();

// Row count: users with more than 10 comments
var c = userService.newCriteria();
var chatty = c
    .subLt( 10, c.subquery( "Comment", "cm" ).eqProperty( "cm.author", "this" ).withProjections( rowCount = true ) )
    .list();

// Id projection: posts whose author is an admin
var c = postService.newCriteria();
var adminPosts = c
    .propertyIn( "author.id", c.subquery( "User", "u" ).isEq( "u.role.slug", "admin" ).withProjections( id = true ) )
    .list();
```

{% hint style="info" %}
**Changes from cborm 5**

* A subquery can't be used as a projection of the outer query: `detachedSQLProjection` has no equivalent. For a computed column such as "number of posts per user", group and count from the other side, or use HQL through `executeQuery()`:

```javascript
// Post count per author, from the Post side
var perAuthor = postService
    .newCriteria()
    .project( ( p ) => p.group( "author.id", "authorId" ).rowCount( "posts" ) )
    .asStruct()
    .list();

// A subquery in the select list, with HQL
var rows = ormService.executeQuery(
    "select u.id as id, ( select count( p ) from Post p where p.author = u ) as posts from User u"
);
```

* You no longer need `this.property` or casts to work around projection aliases in the main criteria: paths are resolved against the entity, and projection aliases only name result columns.
{% endhint %}
