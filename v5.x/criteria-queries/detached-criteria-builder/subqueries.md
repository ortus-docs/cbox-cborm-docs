---
description: "Every condition that takes a subquery: exists, isIn, property*, sub* and the quantified All/Some forms"
---

# Subqueries

A subquery becomes part of the query through a **subquery condition** called on the outer criteria. The subquery is always the last argument.

```javascript
var c = userService.newCriteria();

var users = c
    .isTrue( "isActive" )
    .propertyIn( "id", c.subquery( "Post", "p" ).isTrue( "isPublished" ).withProjections( property = "author.id" ) )
    .list();
```

Subquery conditions are ordinary conditions: they can be negated with the `not` prefix, grouped with `or()`, `and()` and `not()`, and built with `c.restrictions`.

## Existence

| Method                  | Meaning |
| ----------------------- | ------- |
| `exists( subquery )`    | The subquery returns at least one row |
| `notExists( subquery )` | The subquery returns no rows |

```javascript
// Roles with no users
var c = roleService.newCriteria();
var unused = c.notExists( c.subquery( "User", "u" ).eqProperty( "u.role", "this" ) ).list();
```

## Compare a property with a subquery

These compare a property of the outer query with the value the subquery selects (its [projection](projections.md)).

| Method                                   | Meaning |
| ---------------------------------------- | ------- |
| `propertyEq( property, subquery )`       | `property = ( subquery )` |
| `propertyNe( property, subquery )`       | `property <> ( subquery )` |
| `propertyGt( property, subquery )`       | `property > ( subquery )` |
| `propertyGe( property, subquery )`       | `property >= ( subquery )` |
| `propertyLt( property, subquery )`       | `property < ( subquery )` |
| `propertyLe( property, subquery )`       | `property <= ( subquery )` |
| `propertyIn( property, subquery )`       | `property in ( subquery )`. `isIn( property, subquery )` is the same |
| `propertyNotIn( property, subquery )`    | `property not in ( subquery )`. `isNotIn( property, subquery )` is the same |

```javascript
// The most recent post
var c = postService.newCriteria();
var latest = c
    .propertyEq( "publishedDate", c.subquery( "Post", "p2" ).withProjections( max = "publishedDate" ) )
    .get();
```

## Compare a value with a subquery

These compare a value you pass with the value the subquery selects.

| Method                             | Meaning |
| ---------------------------------- | ------- |
| `subEq( value, subquery )`         | `value = ( subquery )` |
| `subNe( value, subquery )`         | `value <> ( subquery )` |
| `subGt( value, subquery )`         | `value > ( subquery )` |
| `subGe( value, subquery )`         | `value >= ( subquery )` |
| `subLt( value, subquery )`         | `value < ( subquery )` |
| `subLe( value, subquery )`         | `value <= ( subquery )` |
| `subIn( value, subquery )`         | `value in ( subquery )` |
| `subNotIn( value, subquery )`      | `value not in ( subquery )` |

```javascript
// Users with at least 5 posts: 5 <= ( select count(*) ... )
var c = userService.newCriteria();
var prolific = c
    .subLe( 5, c.subquery( "Post", "p" ).eqProperty( "p.author", "this" ).withProjections( rowCount = true ) )
    .list();
```

## Quantified comparisons: `All` and `Some`

The quantified forms compare with **every** row (`All`) or **at least one** row (`Some`) of the subquery, for example `price >= all ( select ... )`.

| Compare a property                      | Compare a value                   | Meaning |
| --------------------------------------- | --------------------------------- | ------- |
| `propertyEqAll( property, subquery )`   | `subEqAll( value, subquery )`     | `= all` |
| `propertyGtAll( property, subquery )`   | `subGtAll( value, subquery )`     | `> all` |
| `propertyGtSome( property, subquery )`  | `subGtSome( value, subquery )`    | `> some` |
| `propertyGeAll( property, subquery )`   | `subGeAll( value, subquery )`     | `>= all` |
| `propertyGeSome( property, subquery )`  | `subGeSome( value, subquery )`    | `>= some` |
| `propertyLtAll( property, subquery )`   | `subLtAll( value, subquery )`     | `< all` |
| `propertyLtSome( property, subquery )`  | `subLtSome( value, subquery )`    | `< some` |
| `propertyLeAll( property, subquery )`   | `subLeAll( value, subquery )`     | `<= all` |
| `propertyLeSome( property, subquery )`  | `subLeSome( value, subquery )`    | `<= some` |

There is no `EqSome` form: use `propertyIn()` or `subIn()`, which mean the same.

```javascript
var c = productService.newCriteria();
var bookPrices = c.subquery( "Product", "p" ).isEq( "p.category", "books" ).withProjections( property = "price" );

// Products at least as expensive as every book
var premium = c.propertyGeAll( "price", bookPrices ).list();
```

```javascript
// Is there a book that costs more than 10? 10 < some ( select price ... )
var c = productService.newCriteria();
var hasPricey = c
    .subLtSome( 10, c.subquery( "Product", "p" ).isEq( "p.category", "books" ).withProjections( property = "price" ) )
    .exists();
```

{% hint style="info" %}
**Changes from cborm 5**

In cborm 5 the subquery method was called on the detached criteria and the result was passed to `add()`: `c.add( dc.propertyIn( "id" ) )` or `c.add( dc.subEq( 5 ) )`. In cborm 6 the method is called on the outer criteria with the subquery as its last argument: `c.propertyIn( "id", dc )`, `c.subEq( 5, dc )`. The method names are unchanged.
{% endhint %}
