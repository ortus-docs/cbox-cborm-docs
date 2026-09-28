---
description: "Automatic Java Types - Value conversion is automatic in cborm 6 through bx-orm."
icon: code
---

# Automatic Java Types

In cborm 6 you no longer need to cast values to Java types. The `bx-orm` module converts every value you pass (ids, HQL parameters, dynamic finder arguments, criteria values) to the type of the property it is compared with, so you can pass plain BoxLang values:

```javascript
// No javaCast() needed: bx-orm converts "123" to the id type
var user = ormService.get( "User", "123" );

// Criteria values are converted too
var users = ormService
    .newCriteria( "User" )
    .isEq( "id", rc.id )
    .isGt( "lastLogin", "2024-01-01" )
    .list();
```

{% hint style="warning" %}
**Changed in cborm 6**: `idCast()` and `autoCast()` (and their `convertIdValueToJavaType()` / `convertValueToJavaType()` aliases) are deprecated and no longer cast anything. They are kept only for backwards compatibility:

* [`idCast( entity, id )`](service-methods/utility-methods/idcast.md) normalizes an id, a list of ids or an array of ids into an array of ids.
* [`autoCast( entity, propertyName, value )`](service-methods/utility-methods/autocast.md) returns the value as is.

You can safely remove these calls from your code. Because `idCast()` now returns an array, do not wrap a single id with it (for example `c.isEq( "id", ormService.idCast( "User", id ) )`): pass the id directly. The criteria builder returned by `newCriteria()` no longer has `idCast()` or `autoCast()` methods.
{% endhint %}

## `nullValue()`

The services still give you a [`nullValue()`](service-methods/utility-methods/nullvalue.md) helper that produces a `null` you can use anywhere you like.
