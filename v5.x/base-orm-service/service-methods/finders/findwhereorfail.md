# findWhereOrFail

Find one entity according to a criteria structure, or throw an `EntityNotFound` exception when none matches. Like [findWhere](findwhere.md), more than one match raises `orm.query.nonUnique` unless you pass `{ uniqueFirst : true }` as `options`.

{% hint style="info" %}
The `EntityNotFound` type is the one ColdBox's `RestHandler` turns into a 404, like [getOrFail](../getters/getorfail.md).
{% endhint %}

## Returns

* This function returns the entity

## Arguments

| Key        | Type   | Required | Default | Description                                                         |
| ---------- | ------ | -------- | ------- | ------------------------------------------------------------------- |
| entityName | string | Yes      | ---     | The entity to search                                                |
| criteria   | struct | No       | `{}`    | A structure of property values to filter on                         |
| options    | struct | No       | `{}`    | More `bx-orm` `entityLoad()` options, e.g. `{ uniqueFirst : true }` |

## Examples

```javascript
var user = ormService.findWhereOrFail( "User", { username : rc.username } );
```
