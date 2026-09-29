---
description: "Query on associations with dotted paths, joins, aliases, with{Association}() and fetching"
---

# Associations

You can put conditions on associated entities in several ways:

* **Dotted paths**: `isEq( "role.slug", "admin" )`. The builder joins the association for you.
* **Joins with aliases**: `joinTo( "role", "r" )` and then `isEq( "r.slug", "admin" )`.
* **Scoped conditions**: `withRole( ( r ) => r.isEq( "slug", "admin" ) )` or `createCriteria( "role" )`.

## Dotted paths

A dotted path joins the associations it goes through, for any kind of association (many-to-one, one-to-many, many-to-many, one-to-one), and as deep as you need.

```javascript
// Active users with the admin role
function findAllAdmins(){
    return newCriteria()
        .isTrue( "isActive" )
        .isEq( "role.slug", "admin" )
        .list();
}

// Posts in a category of a given site
postService
    .newCriteria()
    .isEq( "categories.slug", "boxlang" )
    .isEq( "site.slug", "blog" )
    .list();
```

How paths are joined:

* A condition uses an **inner** join, so rows without the association are left out.
* Inside `or()` and `not()` a **left** join is used, so a row without the association can still match another branch.
* Ordering and projections use a **left** join, so ordering never drops rows.
* The same path is joined only once and reused.
* `role.id` compares the foreign key and needs no join.
* When a condition goes through a to-many association, each root entity is still returned once, and `count()` counts each one once.

An association can also be compared directly with an id or an entity:

```javascript
postService.newCriteria().isEq( "author", 42 ).list();
postService.newCriteria().isEq( "author", currentUser ).list();
postService.newCriteria().isIn( "author", [ 1, 42 ] ).list();
```

## Joins and aliases: `joinTo()`

`joinTo( association, alias, [joinType] )` (alias `createAlias()`) joins an association and gives it an alias you can use in later paths. The root entity's alias is `this`.

```javascript
// Using a virtual service
function findAllAdmins(){
    return newCriteria()
        .isTrue( "isActive" )
        .joinTo( "role", "r" )
        .isEq( "r.slug", "admin" )
        .list();
}

// Aliases can be chained
postService
    .newCriteria()
    .joinTo( "author", "a" )
    .joinTo( "a.role", "ar" )
    .isEq( "ar.slug", "editor" )
    .list();
```

| Argument      | Description |
| ------------- | ----------- |
| `association` | The association property, or a dotted path to one (also accepted as `associationName`) |
| `alias`       | The alias to use in later paths |
| `joinType`    | `inner` (default), `left`, `right` or `full`, or one of the cborm constants below |

The join type can be a name or one of the cborm constants on the criteria: `c.INNER_JOIN`, `c.LEFT_JOIN` (or `c.LEFT_OUTER_JOIN`), `c.RIGHT_JOIN` (or `c.RIGHT_OUTER_JOIN`) and `c.FULL_JOIN` (or `c.FULL_OUTER_JOIN`). There are also shortcut methods.

```javascript
c.joinTo( "role", "r", "left" );
c.joinTo( "role", "r", c.LEFT_JOIN );
c.leftJoin( "role", "r" );   // also innerJoin(), rightJoin(), fullJoin()
```

Right and full joins need database support (MySQL and MariaDB have no full join).

## Scoped conditions: `with{Association}()` and `createCriteria()`

`with{Association}( [joinType], [callback] )` joins an association and makes it the start of unqualified paths, so you can write `isEq( "slug", "admin" )` instead of `isEq( "role.slug", "admin" )`.

* With a closure, the scope applies to the conditions the closure adds, then returns to the root entity.
* Without a closure, it applies until you call `end()` (aliases `endAssociation()`, `resetCriteria()`).

`createCriteria( association, [joinType], [callback] )` is the same thing with the association name as an argument.

```javascript
// With a closure
userService
    .newCriteria()
    .like( "firstName", "Lui%" )
    .withRole( ( r ) => r.isEq( "slug", "admin" ) )
    .list();

// Without a closure, until end()
userService
    .newCriteria()
    .withRole( "left" )
        .isEq( "slug", "admin" )
    .end()
    .like( "firstName", "Lui%" )
    .list();

// cborm style
userService
    .newCriteria()
    .like( "firstName", "Lui%" )
    .createCriteria( "role" )
        .isEq( "slug", "admin" )
    .list();
```

## Fetching associations

`fetch( association )` (alias `joinFetch()`) loads an association together with the root entities (a `left join fetch`), so reading it later needs no extra query:

```javascript
var posts = postService.newCriteria().fetch( "author" ).fetch( "categories" ).list();
```

{% hint style="info" %}
**Changes from cborm 5**

* Dotted paths work through every association type, not only many-to-one.
* The `withClause` argument of `joinTo()` and `createCriteria()` (an extra condition in the join's `ON` clause) has no equivalent. For inner joins, add the condition with the alias instead, which returns the same rows. For a left join with an `ON` condition, use HQL through `executeQuery()`.
* `createCriteria()` no longer takes an alias: its second argument is the join type or a closure. Use `joinTo()` when you need an alias.
* After `createCriteria()` or `with{Association}()` you can return to the root entity with `end()`.
{% endhint %}
