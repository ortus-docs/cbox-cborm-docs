---
description: "Every condition method of the criteria builder, groups with or/and/not, and c.restrictions"
---

# Restrictions

Restrictions are the conditions of your query, the `WHERE` clause in SQL terms. You add them by calling condition methods on the criteria, and they build on each other with `and`. Values are always bound as query parameters and converted to the property's type (see [Value Casting](value-casting.md)).

```javascript
var users = userService
    .newCriteria()
    .isTrue( "isActive" )
    .like( "lastName", "Ma%" )
    .between( "age", 18, 65 )
    .isIn( "role.name", [ "admin", "editor" ] )
    .list();
```

## Condition methods

Every condition takes a property path first. Paths can use dots to go through associations (`role.name`, see [Associations](../associations.md)) and can be function calls (`lower(email)`, see [Function paths](#function-paths)).

| Method (aliases)                                                      | Meaning                                                                                              |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `isEq( property, value )` (`eq`)                                      | `property = value`. A `null` value means `property is null`                                          |
| `ne( property, value )` (`isNe`, `isNotEq`, `notEqual`)               | `property <> value`. A `null` value means `property is not null`                                      |
| `isGt( property, value )` (`gt`)                                      | `property > value`                                                                                   |
| `isGe( property, value )` (`ge`, `isGte`, `gte`)                      | `property >= value`                                                                                  |
| `isLt( property, value )` (`lt`)                                      | `property < value`                                                                                   |
| `isLe( property, value )` (`le`, `isLte`, `lte`)                      | `property <= value`                                                                                  |
| `like( property, pattern )` (`isLike`, `whereLike`)                   | `property like pattern`. You include the `%` wildcards                                               |
| `ilike( property, pattern )` (`isIlike`, `whereIlike`)                | Case-insensitive `like`                                                                              |
| `between( property, min, max )` (`isBetween`, `whereBetween`)         | Inclusive range                                                                                      |
| `isIn( property, values )` (`in`, `whereIn`)                          | `values` is an array, a comma-separated list or a [subquery](../../detached-criteria-builder/README.md). An empty list matches nothing |
| `isNotIn( property, values )` (`whereNotIn`)                          | The opposite of `isIn()`                                                                             |
| `isNull( property )` (`whereNull`)                                    | `property is null`                                                                                   |
| `isNotNull( property )` (`whereNotNull`)                              | `property is not null`                                                                               |
| `isTrue( property )`, `isFalse( property )`                           | Boolean properties                                                                                   |
| `isEmpty( property )`, `isNotEmpty( property )`                       | Collections (one-to-many, many-to-many) with no rows, or with at least one                           |
| `sizeEq`, `sizeNe`, `sizeGt`, `sizeGe`, `sizeLt`, `sizeLe( property, size )` | Compare a collection's size                                                                  |
| `eqProperty`, `neProperty`, `gtProperty`, `geProperty`, `ltProperty`, `leProperty( property, otherProperty )` | Compare two properties                                           |
| `idEq( id )`                                                          | Match the entity id. For a composite id, pass a struct: `idEq( { orderId : 1, lineNumber : 2 } )`     |
| `sql( sql, params )` (`sqlRestriction`)                               | A native SQL condition, see [SQL Restrictions](sql-restrictions.md)                                  |
| `exists( subquery )`, `notExists( subquery )`, `property*`, `sub*`    | Subquery conditions, see [Subqueries](../../detached-criteria-builder/subqueries.md)                 |

```javascript
c.between( "age", 10, 30 );
c.isEq( "age", 30 );
c.isGt( "publishedDate", now() );
c.gtProperty( "balance", "overdraft" );
c.idEq( 4 );
c.ilike( "lastName", "maj%" );
c.isIn( "id", [ 1, 2, 3, 4 ] );
c.isEmpty( "comments" );
c.isFalse( "isPublished" );
c.isNull( "passwordProtection" );
c.ne( "status", "banned" );
c.neProperty( "password", "passwordHash" );
c.sizeGe( "comments", 10 );
c.isTrue( "isActive" );
```

Arguments can also be passed by name. Each method accepts its cborm argument names: `property` (or `propertyName`), `value` (or `propertyValue`), `minValue`/`maxValue` for `between()`, `otherProperty` for the property comparisons.

```javascript
c.isEq( property = "lastName", value = "Majano" );
c.between( property = "age", minValue = 18, maxValue = 65 );
```

## `where()` shorthands

`where()` is a compact way to write the most common conditions:

```javascript
c.where( "lastName", "Majano" );                    // isEq
c.where( "age", ">=", 18 );                          // =, !=, <>, >, >=, <, <=, like, ilike, in, not in
c.where( { lastName : "Majano", isActive : true } ); // several isEq; a null value means is null
c.where( ( c ) => c.isEq( "a", 1 ).isEq( "b", 2 ) ); // an and-group
```

## Negation

Every condition can be negated by prefixing its name with `not`:

```javascript
c.notEq( "status", "banned" )
    .notIn( "id", [ 1, 2, 3 ] )
    .notLike( "email", "%@example.com" )
    .notBetween( "age", 18, 21 )
    .notIsNull( "lastLogin" )
    .notEmpty( "posts" );
```

To negate a group of conditions, use `not()` (alias `isNot()`) with a closure or a restriction:

```javascript
// not ( lastName = 'Majano' and firstName = 'Luis' )
c.not( ( c ) => c.isEq( "lastName", "Majano" ).isEq( "firstName", "Luis" ) );

// not ( status = 'banned' )
c.not( c.restrictions.isEq( "status", "banned" ) );
```

## Groups: `or()` and `and()`

Conditions are joined with `and` by default. `or()` joins the conditions of its arguments with `or`, and `and()` joins them with `and`, which is useful inside an `or()`. Each argument is a restriction (see below) or a closure that receives the criteria.

```javascript
// firstName = 'Luis' or lastName = 'Majano'
c.or( c.restrictions.isEq( "firstName", "Luis" ), c.restrictions.isEq( "lastName", "Majano" ) );

// The same with a closure: its conditions are joined with or
c.or( ( c ) => c.isEq( "firstName", "Luis" ).isEq( "lastName", "Majano" ) );

// ( firstName = 'Luis' and lastName like 'M%' ) or age < 30
c.or( ( c ) => c
    .and( ( a ) => a.isEq( "firstName", "Luis" ).like( "lastName", "M%" ) )
    .isLt( "age", 30 )
);
```

When you pass several closures to `or()`, each closure is one alternative and its own conditions must all match:

```javascript
// ( role = admin and isActive ) or ( role = editor )
c.or(
    ( c ) => c.isEq( "role.name", "admin" ).isTrue( "isActive" ),
    ( c ) => c.isEq( "role.name", "editor" )
);
```

Aliases: `$or`, `anyOf`, `orWhere` and `disjunction` for `or()`; `$and`, `allOf` and `conjunction` for `and()`.

## `c.restrictions` and `getRestrictions()`

`c.restrictions` builds a condition **without adding it** to the criteria: the cborm way of preparing conditions for `add()`, `or()`, `and()` and `not()`. It has every condition method above, with the same names and aliases, including the `not...` forms, plus `or()`, `and()` and `not()` to nest them.

The services give you the same object through `getRestrictions()`: see [getRestrictions()](../../../base-orm-service/service-methods/criteria-queries/getrestrictions.md).

```javascript
var c = userService.newCriteria();
var r = c.restrictions; // or userService.getRestrictions()

c.add( r.like( "firstName", "A%" ), r.isNotNull( "email" ) );
c.or( r.isEq( "role.name", "admin" ), r.isGt( "age", 30 ) );
c.not( r.isEq( "status", "banned" ) );
c.add( r.or( r.isEq( "a", 1 ), r.and( r.isEq( "b", 2 ), r.not( r.isNull( "c" ) ) ) ) );

// Restrictions and closures can be mixed
c.or( r.isEq( "a", 1 ), ( c ) => c.isEq( "b", 2 ) );
```

A restriction is resolved against the criteria it is added to, so restrictions are not tied to an entity. Calling a method that is not a condition, such as `c.restrictions.list()`, is an `orm.argument` error.

`add( restriction, ... )` adds one or more restrictions (or closures) with `and`:

```javascript
c.add( c.restrictions.isEq( "firstName", "Luis" ) );
```

{% hint style="info" %}
**Changes from cborm 5**

* Restrictions are no longer Hibernate `Criterion` objects, and methods that are not listed here are not proxied to Hibernate's `Restrictions` class any more.
* `and()`, `or()`, `conjunction()` and `disjunction()` take restrictions or closures as separate arguments, not an array. To combine an array of restrictions, add them inside a closure: `c.or( ( g ) => myRestrictions.each( ( r ) => g.add( r ) ) )`.
* Values no longer need `javaCast()`, `idCast()` or `autoCast()`: see [Value Casting](value-casting.md).
{% endhint %}

## Function paths

Any property argument can be a function call, in conditions, in `order()` and in projections. This often replaces a native `sql()` restriction.

```javascript
c.isEq( "year(createdDate)", 2025 );
c.isEq( "lower(email)", "luis@example.com" );
c.isEq( "upper(substring(lastName, 1, 3))", "MAJ" );
c.isEq( "coalesce(nickname, 'none')", "none" );
c.isEq( "lower(role.name)", "admin" ); // paths join as usual
c.order( "length(lastName) desc" );
```

* Each function argument must be a property path, a nested function call, a number or a `'quoted string'` (`''` escapes a quote). Pass anything else as the condition value, where it is bound as a parameter.
* The function can be an HQL function (`lower`, `upper`, `length`, `substring`, `coalesce`, `year`, `cast`, ...), a function of the database dialect, or one of the application's named SQL functions (bx-orm `sqlFunctions` setting).
* An unknown function name is passed to the database, which rejects it when the query runs.

## Conditional building: `when()` and `unless()`

Instead of wrapping criteria calls in `if` statements, use `when( test, callback, [otherwise] )`. When the test is true, the callback is called with the criteria; when it is false, the optional `otherwise` callback is. `unless()` is the opposite. The test can also be a closure that receives the criteria and returns a boolean.

```javascript
var posts = postService
    .newCriteria()
    .when( !isNull( arguments.isPublished ), ( c ) => {
        c.isEq( "isPublished", isPublished )
            .when( isPublished, ( c ) => {
                c.isLt( "publishedDate", now() )
                    .or(
                        c.restrictions.isNull( "expireDate" ),
                        c.restrictions.isGt( "expireDate", now() )
                    )
                    .isEq( "passwordProtection", "" );
            } );
    } )
    .when( !isNull( arguments.showInSearch ), ( c ) => c.isEq( "showInSearch", showInSearch ) )
    .when( arguments.isActive, ( c ) => c.isTrue( "isActive" ), ( c ) => c.isFalse( "isActive" ) )
    .unless( arguments.showDeleted, ( c ) => c.isFalse( "isDeleted" ) )
    .list();
```

`apply( callback )` (alias `scope()`) calls a reusable closure with the criteria, which is handy for shared filters:

```javascript
var onlyActive = ( c ) => c.isTrue( "isActive" ).isNull( "deletedDate" );

userService.newCriteria().apply( onlyActive ).like( "lastName", "M%" ).list();
```
