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

The property names in a dynamic method are matched case-insensitively against the entity's properties, and the compiled HQL uses the declared case. So `findAllByLastName()`, `findAllBylastname()` and `findAllByLASTNAME()` are the same finder. The `sortBy` query option is normalized the same way.

## Streams

We have also enabled the ability to return a stream of objects if you are using the `findAll` semantics via [cbStreams](https://forgebox.io/view/cbStreams).

```javascript
userStream = getInstance( "User" )
    .findAllByLastLoginGreaterThan( "01/01/2010", { asStream : true } );
```
