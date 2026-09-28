---
description: "Build ORM queries fluently with newCriteria(), the bx-orm criteria builder"
icon: filters
---

# Criteria Builder

The criteria builder lets you build a query one method at a time instead of writing an HQL string. You chain conditions, joins, projections and ordering, then run it with a terminal method such as `list()`, `count()` or `get()`. Property names are checked as you add them and every value is bound as a query parameter, so there is no string concatenation and no SQL injection risk.

In cborm 6, `newCriteria()` returns the criteria builder of the [bx-orm](https://forgebox.io/view/bx-orm) module: the same object BoxLang's `entityCriteria()` function returns. It keeps the cborm method names you already know (`isEq`, `like`, `between`, `isIn`, `joinTo`, `withProjections`, `list`, `count`, `get`, `getOrFail`, ...) and adds many more (`paginate()`, `pluck()`, `each()`, `chunk()`, `updateAll()`, `deleteAll()`, `lock()`, function paths, ...).

```javascript
// Active users named Luis
userService
    .newCriteria()
    .isEq( "firstName", "Luis" )
    .isTrue( "isActive" )
    .getOrFail();

// Active admins, as a Java stream
userService
    .newCriteria()
    .isTrue( "isActive" )
    .isEq( "role.name", "admin" )
    .asStream()
    .list();

// Only a few columns, as an array of structs
userService
    .newCriteria()
    .withProjections( property = "id,firstName:fname,lastName:lname,age" )
    .isTrue( "isActive" )
    .joinTo( "role", "r" )
    .isEq( "r.name", "admin" )
    .asStruct()
    .list();
```

## How it works

* **Building methods** (conditions, joins, ordering, options) change the criteria and return it, so they chain.
* **Terminal methods** (`list()`, `count()`, `get()`, `paginate()`, ...) run one query and return its result. They never change the criteria, so the same criteria can run `count()` and then `list()`, in any order.
* `copy()` returns an independent copy to branch from.
* Method names are case-insensitive, and arguments can be positional or named: `isEq( "lastName", "Majano" )` or `isEq( property = "lastName", value = "Majano" )`.
* Property names are case-insensitive too: the builder resolves each path against the entity's mapping and fixes its casing. A misspelled property fails at once with a suggestion, for example `User has no property [fristName]. Did you mean [firstName]?`

{% hint style="success" %}
**Tip**: You don't have to use the ORM for everything. Please be pragmatic. If you can't figure a query out in 10 minutes or less, move to HQL or direct SQL.
{% endhint %}

## Changes from cborm 5

{% hint style="info" %}
Hibernate 7 removed the legacy Criteria API (`org.hibernate.Criteria`, `Restrictions`, `DetachedCriteria`, `Projections`, `org.hibernate.criterion.*`), so cborm 5's `CriteriaBuilder` and `DetachedCriteriaBuilder` wrappers are gone. `newCriteria()` now returns the bx-orm builder, which compiles to HQL. Most cborm code keeps working unchanged. The main differences:

* `get( properties )` becomes `withProjections( property = "..." ).asStruct().get()`.
* `createSubcriteria()` and the Detached Criteria Builder become `c.subquery( "Entity", "alias" )`: see [Subqueries](../detached-criteria-builder/README.md).
* `cacheRegion( name )` becomes `cache( true, name )`.
* `asStream()` returns a Java stream instead of a cbStreams stream.
* `idCast()`, `autoCast()` and typed SQL parameters are no longer needed: bx-orm converts values itself.
* `getNativeCriteria()`, `resultTransformer()`, `setProjection()`, `c.projections`, SQL projections and `getPositionalSQLParameters()` no longer exist.

Adobe ColdFusion and Lucee are not supported by cborm 6: stay on the cborm 5.x series for those engines.
{% endhint %}

## Where to go next

* [Getting Started](getting-started.md): get a criteria and run your first queries.
* [Restrictions](restrictions/README.md): every condition method, groups (`or`, `and`, `not`) and `c.restrictions`.
* [Associations](associations.md): dotted paths, joins, aliases and fetching.
* [Projections & Aggregates](projections.md): select columns, group, count and sum.
* [Modifiers](modifiers.md) and [Results](results.md): ordering, paging, caching, locking, result shapes and terminal methods.
* [SQL Log & Debugging](sql-log.md) and [Interception Events](interception-events.md).
