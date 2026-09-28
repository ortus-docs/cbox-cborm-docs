# autoCast

{% hint style="warning" %}
**Changed in cborm 6**: this method is deprecated and no longer casts anything, it returns the value as is. `bx-orm` converts query values to the property's type itself, so you can remove these calls. The `convertValueToJavaType()` alias behaves the same way. See [Automatic Java Types](../../automatic-java-types.md).
{% endhint %}

## Returns

* This function returns the `value` untouched

## Arguments

| Key          | Type   | Required | Default | Description                               |
| ------------ | ------ | -------- | ------- | ----------------------------------------- |
| entity       | any    | Yes      |         | The entity name or entity object (unused) |
| propertyName | string | Yes      |         | The property name (unused)                |
| value        | any    | Yes      |         | The property value                        |

## Examples

```javascript
// Returns arguments.myDate as is
baseService.autoCast( "User", "createdDate", arguments.myDate );
```
