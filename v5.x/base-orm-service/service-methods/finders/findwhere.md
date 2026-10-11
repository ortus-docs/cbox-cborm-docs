# findWhere

Find one entity (or `null` if not found) according to a criteria structure. Ex: `findWhere( "Category", { category : "Training" } )`, `findWhere( "User", { age : 40, retired : false } )`

{% hint style="warning" %}
**Changed in cborm 6**: more than one match raises an `orm.query.nonUnique` error, with or without query caching. Pass `{ uniqueFirst : true }` as `options` to take the first match instead.
{% endhint %}

## Returns

* This function returns _any_: the entity or `null`

## Arguments

| Key        | Type   | Required | Default | Description                                                                                                                  |
| ---------- | ------ | -------- | ------- | ---------------------------------------------------------------------------------------------------------------------------- |
| entityName | string | Yes      | ---     | The entity to search                                                                                                         |
| criteria   | struct | No       | `{}`    | A structure of property values to filter on. A `null` value matches `is null`, and each key must be a property of the entity |
| options    | struct | No       | `{}`    | More `bx-orm` `entityLoad()` options, e.g. `{ uniqueFirst : true, readOnly : true }`                                         |

## Examples

```javascript
// Find a category according to the named value pairs I pass into this method
var category = ormService.findWhere( entityName = "Category", criteria = { isActive : true, label : "Training" } );
var user     = ormService.findWhere( entityName = "User", criteria = { isActive : true, username : rc.username } );

// The first match when several can match
var newest = ormService.findWhere( "Post", { author : "Luis Majano" }, { uniqueFirst : true } );
```

See also [findWhereOrFail](findwhereorfail.md), [firstOrNew](firstornew.md) and [firstOrCreate](firstorcreate.md).
