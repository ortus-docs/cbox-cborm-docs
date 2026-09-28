# idCast

{% hint style="warning" %}
**Changed in cborm 6**: this method is deprecated and no longer casts identifiers to Java types, since `bx-orm` converts id values to the entity's id type itself. It only normalizes an id, a list of ids or an array of ids into an **array** of ids. The `convertIdValueToJavaType()` alias behaves the same way. See [Automatic Java Types](../../automatic-java-types.md).
{% endhint %}

## Returns

* This function returns an array of ids

## Arguments

| Key    | Type | Required | Default | Description                               |
| ------ | ---- | -------- | ------- | ----------------------------------------- |
| entity | any  | Yes      |         | The entity name or entity object (unused) |
| id     | any  | Yes      |         | The id, list of ids or array of ids       |

## Examples

```javascript
// [ "123" ]
baseService.idCast( "User", "123" );
// [ "1", "2", "3" ]
baseService.idCast( "User", "1,2,3" );
```
