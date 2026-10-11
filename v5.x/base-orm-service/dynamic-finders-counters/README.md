---
description: "Dynamic Finders and Counters - Utilize dynamic methods for querying ORM entities."
icon: radar
---

# Dynamic Finders- Counters

The ORM module supports the concept of dynamic finders and counters for ORM entities. A dynamic finder/counter looks like a real method but it is a virtual method that is intercepted via `onMissingMethod()` and compiled to HQL. This is a great way for you to do finders and counters using a programmatic and visual representation of what HQL to run.

This feature works on the Base ORM Service, Virtual Entity Services and also Active Entity services. The most semantic and clear representations occur in the Virtual Entity Service and Active Entity as you don't have to pass an entity name around.

```java
users = getInstance( "User" )
    .findAllByLastLoginGreaterThan( "01/01/2010" );

users = getInstance( "User" )
    .findAllByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );

count = getInstance( "User" )
    .countByLastLoginGreaterThan( "01/01/2010" );

count = getInstance( "User" )
    .countByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );
```

## Automatic Casting

You don't have to worry about Java types: `bx-orm` converts every value you pass to the type of the property it is compared with.

## Property Names in Any Case

The property names in a dynamic method are matched case-insensitively against the entity's properties, and the compiled HQL uses the declared case. So `findAllByLastName()`, `findAllBylastname()` and `findAllByLASTNAME()` are the same finder. The id properties are part of the grammar too: `findAllByIdInList( "1,2,3" )`.

## Compiled Once

Each finder is compiled to HQL once per entity and method, from `bx-orm`'s entity metadata, and cached. If an `ormReload()` changes an entity so a cached finder no longer matches, it is compiled again automatically. You can also clear the cache yourself: `getInstance( "cborm.models.util.DynamicProcessor" ).clearCache()`.

## Errors

* A grammar that names no known property raises `InvalidMethodGrammar`, and an unknown property raises `InvalidEntityProperty`.
* A `findBy` that matches more than one row raises `NonUniqueResultException`: use `findAllBy` instead.
* Query errors are `bx-orm`'s typed `orm.*` errors (for example `orm.query.parameter` when a value is missing), with their "Did you mean" hints. cborm 5 wrapped them all in `HQLQueryException`.

## Streams

Pass `asStream : true` as a query option to get a Java `Stream` read from the database as it is consumed (see [Java Streams](../../advanced/java-streams.md)). A `findBy` finder returns a stream of zero or one entity.

```javascript
userStream = getInstance( "User" )
    .findAllByLastLoginGreaterThan( "01/01/2010", { asStream : true } );
```
