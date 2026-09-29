---
description: "Get a new criteria builder (a bx-orm entityCriteria()) bound to an entity"
---

# newCriteria

Get a brand new criteria builder to build and run criteria queries with. In cborm 6 it is a bx-orm criteria builder, the same object `entityCriteria( entityName )` returns. See [Criteria Builder](../../../criteria-queries/criteria-builder/getting-started.md).

## Returns

* The bx-orm criteria builder for the entity (`entityCriteria()`)

## Arguments

| Key              | Type    | Required | Default                | Description |
| ---------------- | ------- | -------- | ---------------------- | ----------- |
| entityName       | string  | true     | ---                    | The entity name to bind the criteria query to. Not passed on a Virtual Entity Service or an Active Entity |
| useQueryCaching  | boolean | false    | false                  | Cache the results of the criteria's queries in the second-level query cache |
| queryCacheRegion | string  | false    | criterias.{entityName} | The query cache region to use when `useQueryCaching` is true |
| datasource       | string  | false    | The service datasource | Ignored: the criteria runs on the entity's own datasource |

## Examples

```javascript
var users = ormService
    .newCriteria( entityName = "User" )
    .isGt( "age", 30 )
    .isTrue( "isActive" )
    .list( max = 30, offset = 10, sortOrder = "lastName" );

// Virtual entity service or Active Entity: no entity name
var admins = userService.newCriteria().isEq( "role.slug", "admin" ).list();

// With query caching in the default region criterias.User
var active = ormService.newCriteria( "User", true ).isTrue( "isActive" ).list();
```

{% hint style="info" %}
**Changes from cborm 5**: `newCriteria()` used to return `cborm.models.criterion.CriteriaBuilder`, a wrapper around the Hibernate Criteria API that Hibernate 7 removed. It now returns the bx-orm criteria builder, which keeps the cborm method names. Values no longer need `idCast()` or `autoCast()`.
{% endhint %}
