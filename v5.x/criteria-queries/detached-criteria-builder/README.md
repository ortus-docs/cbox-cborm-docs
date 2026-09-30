---
description: "Subqueries in criteria queries: c.subquery() replaces the cborm 5 Detached Criteria Builder"
icon: plug-circle-xmark
---

# Subqueries

A subquery is a query inside another query: "users who wrote a post in the last week", "posts more expensive than every book", "roles with no users". In cborm 6 you create one from a criteria with `c.subquery( entityName, alias )` and pass it to a subquery condition of the same criteria (`exists()`, `isIn()`, `propertyIn()`, `subGe()`, ...).

```javascript
// Users who wrote a published post
var c = userService.newCriteria();
var authors = c
    .exists(
        c.subquery( "Post", "p" )
            .eqProperty( "p.author", "this" )
            .isTrue( "isPublished" )
    )
    .list();

// Users with a draft post: their id is in the subquery's projected values
var c = userService.newCriteria();
var drafters = c
    .isIn(
        "id",
        c.subquery( "Post", "p" )
            .isEq( "status", "draft" )
            .withProjections( property = "author.id" )
    )
    .list();
```

A subquery is a criteria builder too: it has the same conditions, joins and projections as the main criteria. It can't be run on its own: calling `list()`, `count()` or `get()` on it is an `orm.argument` error.

* [Getting Started](getting-started.md): create a subquery, correlate it with the outer query, add conditions and joins.
* [Projections](projections.md): choose what the subquery selects.
* [Subqueries](subqueries.md): every condition that takes a subquery, including the quantified `All`/`Some` forms.

{% hint style="info" %}
**Changes from cborm 5**

Hibernate 7 removed `DetachedCriteria`, so cborm's `DetachedCriteriaBuilder` is gone. `c.subquery()` replaces it (`createSubcriteria()` and `detachedCriteria()` are aliases), and the subquery conditions keep their cborm names (`propertyIn`, `subEq`, `subGeAll`, `propertyLtSome`, `exists`, ...). The call order changed: the subquery is now an argument of the condition, added to the outer criteria, instead of a condition called on the subquery and passed to `add()`.

```javascript
// cborm 5
c.add(
    c.createSubcriteria( "Post", "p" )
        .withProjections( property = "author.id" )
        .isEq( "status", "draft" )
        .propertyIn( "id" )
);

// cborm 6
c.propertyIn(
    "id",
    c.subquery( "Post", "p" )
        .withProjections( property = "author.id" )
        .isEq( "status", "draft" )
);
```

Detached criteria can no longer be executed in another session, and `detachedSQLProjection` has no equivalent: see [Projections](projections.md).
{% endhint %}
