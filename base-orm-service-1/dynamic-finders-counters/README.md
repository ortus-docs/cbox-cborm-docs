# Dynamic Finders- Counters

The ORM module supports the concept of dynamic finders and counters for ColdFusion ORM entities. A dynamic finder/counter looks like a real method but it is a virtual method that is intercepted by via `onMissingMethod`. This is a great way for you to do finders and counters using a programmatic and visual representation of what HQL to run.

This feature works on the Base ORM Service, Virtual Entity Services and also Active Entity services. The most semantic and clear representations occur in the Virtual Entity Service and Active Entity as you don't have to pass an entity name around.

```java
users = getInstance( "User" )
    .findAllByLastLoginBetweeninGreaterThan( "01/01/2010" );

users = getInstance( "User" )
    .findAllByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );

count = getInstance( "User" )
    .countByLastLoginGreaterThan( "01/01/2010" );

count = getInstance( "User" )
    .countByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );
```

### Automatic Casting

Another important aspect of the dynamic finders is that we will AUTO CAST all the values for you.  So you don't have to mess with the right Java type, we will do it for you.

### Streams

We have also enabled the ability to return a stream of objects if you are using the `findAll` semantics via [cbStreams](https://forgebox.io/view/cbStreams).

```javascript
userStream = getInstance( "User" )
    .findAllByLastLoginBetweeninGreaterThan( "01/01/2010", {asStream:true} );
```

