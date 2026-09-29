---
description: "Create a subquery with c.subquery(), correlate it with the outer query and add conditions and joins"
---

# Getting Started

## Creating a subquery

Call `subquery( entityName, [alias="sub"] )` on the criteria the subquery belongs to. `createSubcriteria()` and `detachedCriteria()` are aliases, so cborm 5 calls keep working.

| Argument     | Type   | Required | Default | Description |
| ------------ | ------ | -------- | ------- | ----------- |
| `entityName` | string | true     | ---     | The entity the subquery selects from |
| `alias`      | string | false    | `sub`   | The alias of the subquery's entity, for paths like `p.author` |

```javascript
var c = userService.newCriteria();

// A subquery on Post, aliased p
var posts = c.subquery( "Post", "p" );

// cborm 5 name, same thing
var posts = c.createSubcriteria( entityName = "Post", alias = "p" );
```

Then add conditions and a projection to the subquery, and pass it to a subquery condition of the outer criteria:

```javascript
var c = userService.newCriteria();

var users = c
    .propertyIn(
        "id",
        c.subquery( "Post", "p" )
            .isEq( "status", "draft" )
            .withProjections( property = "author.id" )
    )
    .list();
```

Create the subquery from the criteria that uses it (its `this` paths point at that criteria's root), and don't run it on its own: `list()`, `count()` or `get()` on a subquery is an `orm.argument` error.

## Paths inside a subquery

* Unqualified paths start at the subquery's entity: `isEq( "status", "draft" )` is the post's status.
* The subquery's alias works too: `p.status`.
* `this` is the outer query's root entity, and `this.property` one of its properties. This is how you correlate a subquery with the outer query.

```javascript
var c = userService.newCriteria();

// Users with at least one post in the last 7 days
var active = c
    .exists(
        c.subquery( "Post", "p" )
            .eqProperty( "p.author", "this" )
            .isGe( "publishedDate", dateAdd( "d", -7, now() ) )
    )
    .list();

// The same correlation, comparing ids
var c = userService.newCriteria();
var active = c
    .exists(
        c.subquery( "Post" )
            .eqProperty( "author.id", "this.id" )
            .isGe( "publishedDate", dateAdd( "d", -7, now() ) )
    )
    .list();
```

## Conditions and joins

Every condition of the criteria builder works in a subquery: comparisons, `like`, `isIn`, `isNull`, the `not` prefix, `or()`, `and()`, `not()`, `c.restrictions`, `sql()` and function paths. See [Restrictions](../criteria-builder/restrictions/README.md).

Associations work the same way too: dotted paths join automatically, and `joinTo()` (alias `createAlias()`), `leftJoin()`, `with{Association}()` and `createCriteria()` are available. See [Associations](../criteria-builder/associations.md).

```javascript
var c = userService.newCriteria();

// Users who commented on a post in the "boxlang" category
var users = c
    .propertyIn(
        "id",
        c.subquery( "Comment", "cm" )
            .joinTo( "cm.post", "p" )
            .isEq( "p.categories.slug", "boxlang" )
            .withProjections( property = "author.id" )
    )
    .list();

// Subqueries can be combined with any other condition
var c = userService.newCriteria();
var users = c
    .isTrue( "isActive" )
    .or(
        c.restrictions.isEq( "role.slug", "admin" ),
        c.restrictions.exists( c.subquery( "Post", "p" ).eqProperty( "p.author", "this" ) )
    )
    .list();
```

{% hint style="info" %}
**Changes from cborm 5**

* In cborm 5 the subquery was a separate `DetachedCriteriaBuilder` object. Now it is a criteria builder bound to its outer criteria, with the same methods.
* Use `this.property` to reach the outer query's root. The cborm 5 `{alias}.property` convention is only used in [SQL restrictions](../criteria-builder/restrictions/sql-restrictions.md).
{% endhint %}
