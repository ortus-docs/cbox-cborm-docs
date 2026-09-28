---
description: "Select columns, group, count, sum and average with withProjections() and project()"
---

# Projections & Aggregates

Projections select columns instead of whole entities: property values, counts, sums, averages, minimums, maximums and groupings. Combined with `asStruct()`, they return an array of structs with only the data you need, which is ideal for API responses and reports.

```javascript
var statusReport = userService
    .newCriteria()
    .withProjections( groupProperty = "isActive", count = "id:total" )
    .asStruct()
    .list();
// [ { isActive : true, total : 120 }, { isActive : false, total : 8 } ]
```

## `withProjections()`

`withProjections()` is the cborm way to declare projections. Pass named arguments (or one struct) whose values are a property name, a comma-separated list or an array of them. Each entry can take an alias after a colon: `property:alias`.

| Argument        | Projection | Example |
| --------------- | ---------- | ------- |
| `property`      | The property value | `withProjections( property = "id,firstName,lastName" )` |
| `groupProperty` (`group`) | The property value, grouped by (`group by`) | `withProjections( groupProperty = "country" )` |
| `count`         | `count( property )` | `withProjections( count = "id:total" )` |
| `countDistinct` | `count( distinct property )` | `withProjections( countDistinct = "city:cities" )` |
| `distinct`      | The property value, with `select distinct` | `withProjections( distinct = "city" )` |
| `sum`           | `sum( property )` | `withProjections( sum = "salary:payroll" )` |
| `avg`           | `avg( property )` | `withProjections( avg = "salary" )` |
| `min`           | `min( property )` | `withProjections( min = "lastLogin" )` |
| `max`           | `max( property )` | `withProjections( max = "lastLogin" )` |
| `id`            | The id. `true`, or an alias | `withProjections( id = true )` |
| `rowCount`      | `count( * )`. `true`, or an alias | `withProjections( rowCount = true )` |

### Aliases

The alias is the name of the column in `asStruct()` and `asQuery()` results. Without one, the alias is the property name (the last segment of a dotted path), `count` for `rowCount` and the id name for `id`. When two projections would get the same name, the later one is prefixed with its function, for example `maxSalary`.

```javascript
withProjections( avg = "balance" )               // balance
withProjections( avg = "balance:avgBalance" )    // avgBalance
withProjections( avg = "balance,total" )         // balance, total
withProjections( avg = [ "balance", "total" ] )  // balance, total
withProjections( property = "id,firstName:fname,lastName:lname" )
```

Projection aliases name result columns only: they cannot be used in conditions (there is no `having`).

### Examples

```javascript
// Row count with conditions
var total = userService
    .newCriteria()
    .like( "firstName", "Lui%" )
    .withProjections( rowCount = true )
    .get();

// Several aggregates in one row
var stats = orderService
    .newCriteria()
    .isEq( "status", "paid" )
    .withProjections( avg = "total:average", max = "total:largest", rowCount = "orders" )
    .asStruct()
    .get();

// Users per country
var perCountry = userService
    .newCriteria()
    .withProjections( groupProperty = "country", count = "id:users" )
    .asStruct()
    .list();

// Ids of users who never logged in
var ids = userService
    .newCriteria()
    .isNull( "lastLogin" )
    .withProjections( id = true )
    .list();

// Properties through an association
var settings = settingService
    .newCriteria()
    .isFalse( "isDeleted" )
    .isTrue( "isCore" )
    .withProjections( property = "name,site.slug:siteSlug" )
    .asStruct()
    .list( sortOrder = "site.slug,name" );
```

## `project()`

`project( callback )` is the bx-orm way to declare projections. The closure receives a projection object whose methods take a property and an optional alias, and the columns follow the order of the calls.

| Method | Projection |
| ------ | ---------- |
| `property( property, [alias] )` | The property value |
| `group( property, [alias] )` (`groupProperty`) | The property value, grouped by |
| `sum`, `avg`, `min`, `max`, `count`, `countDistinct( property, [alias] )` | Aggregates |
| `rowCount( [alias] )` | `count( * )` |
| `id( [alias] )` | The id |

```javascript
var perRole = userService
    .newCriteria()
    .isTrue( "isActive" )
    .project( ( p ) => p.group( "role.name", "role" ).count( "id", "users" ).max( "lastLogin" ) )
    .asStruct()
    .list();
```

Property arguments can be [function paths](restrictions/README.md#function-paths), which replaces most SQL projections:

```javascript
var perYear = postService
    .newCriteria()
    .project( ( p ) => p.group( "year(publishedDate)", "year" ).rowCount( "posts" ) )
    .asStruct()
    .list();
```

## Result shapes

* With one projection, `list()` returns plain values and `get()` returns one value.
* With several projections, `list()` returns one array per row, in projection order.
* With `asStruct()`, each row is a struct keyed by the aliases. With `asQuery()`, a query with one column per alias.

For a single aggregate, the terminal methods `count()`, `sum()`, `avg()`, `min()`, `max()` and `pluck()` are shorter: see [Results](results.md#aggregates).

{% hint style="info" %}
**Changes from cborm 5**

* `sqlProjection`, `sqlGroupProjection` and `detachedSQLProjection` no longer exist. Use a function path in a projection (`p.group( "year(publishedDate)", "year" )`), or HQL through `executeQuery()` for anything a function path can't express, such as a subquery in the select list.
* `setProjection()`, `c.projections` and `resultTransformer()` no longer exist: use `withProjections()` or `project()`, and `asStruct()`.
* Projection aliases can no longer be used in restrictions.
* The value of `id` and `rowCount` can be an alias instead of `true`.
{% endhint %}
