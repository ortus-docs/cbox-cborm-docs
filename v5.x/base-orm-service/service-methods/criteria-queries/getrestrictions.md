---
description: "Get the restrictions builder that prepares conditions for a criteria's add(), or(), and() and not()"
---

# getRestrictions

Get the restrictions builder. Its methods (`isEq`, `like`, `between`, `isNull`, `or`, `and`, `not`, ...) build conditions **without adding them** to a criteria, so you can pass them to a criteria's `add()`, `or()`, `and()` and `not()`. It is the same object as a criteria's `c.restrictions`. See [Restrictions](../../../criteria-queries/criteria-builder/restrictions/README.md).

## Returns

* The bx-orm criteria restrictions builder

## Arguments

| Key        | Type   | Required | Default | Description |
| ---------- | ------ | -------- | ------- | ----------- |
| entityName | string | false    | ""      | The entity to take the restrictions from. Any mapped entity works, since restrictions are not tied to an entity: they are resolved against the criteria they are added to. When empty, the first mapped entity is used. Not available on a Virtual Entity Service or an Active Entity |

If the application has no ORM entities, a `BaseORMService.NoEntitiesFound` error is thrown.

## Examples

```javascript
var r = ormService.getRestrictions();

var users = ormService
    .newCriteria( "User" )
    .or( r.isEq( "lastName", "Majano" ), r.isGt( "createdDate", dateAdd( "d", -7, now() ) ) )
    .list();

// Nested groups
var users = ormService
    .newCriteria( "User" )
    .add( r.or( r.isEq( "role.slug", "admin" ), r.and( r.isTrue( "isActive" ), r.not( r.isNull( "lastLogin" ) ) ) ) )
    .list();

// Virtual entity service
var r = userService.getRestrictions();
```

{% hint style="info" %}
**Changes from cborm 5**: `getRestrictions()` used to return `cborm.models.criterion.Restrictions`, a proxy to Hibernate's `org.hibernate.criterion.Restrictions`, which Hibernate 7 removed. It now returns the bx-orm restrictions builder, and methods it doesn't have are no longer proxied to Hibernate. The Adobe ColdFusion reserved-word workarounds (`$and()`, `$or()`) are no longer needed, although they still work.
{% endhint %}
