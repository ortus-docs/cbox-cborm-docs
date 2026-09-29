---
description: "How criteria values are converted to the property types in cborm 6"
---

# Value Casting

You no longer need to cast values for criteria queries. bx-orm binds every value as a query parameter and converts it to the type of the property it is compared with, so BoxLang strings, numbers, booleans and dates just work:

```javascript
// A numeric id compared with a string, a date with a BoxLang date
userService
    .newCriteria()
    .isEq( "id", "42" )
    .isGt( "lastLogin", dateAdd( "d", -30, now() ) )
    .isEq( "isActive", true )
    .list();

// Associations can be compared with an id or an entity
postService.newCriteria().isEq( "author", 42 ).list();
postService.newCriteria().isEq( "author", currentUser ).list();
postService.newCriteria().isIn( "author", [ 1, 42 ] ).list();
```

To match a `null`, use `isNull()`, or pass `null` to `isEq()` (which becomes `is null`) or to `ne()` (which becomes `is not null`).

{% hint style="info" %}
**Changes from cborm 5**

* `c.idCast()`, `c.autoCast()` and `c.nullValue()` no longer exist on the criteria builder, and `javaCast()` is not needed for criteria values. Remove the calls and pass the values as they are.
* The services still have `idCast()` and `autoCast()` for compatibility, but they no longer convert to Java types: `idCast()` only normalizes an id, list or array to an array, and `autoCast()` returns the value as is.
* For native SQL conditions, see [SQL Restrictions](sql-restrictions.md): the typed `{ value, type }` parameters are gone too.
{% endhint %}
