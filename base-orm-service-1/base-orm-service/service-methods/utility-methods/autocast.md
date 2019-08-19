# autoCast

This method allows you to cast any value to the appropriate type in Java for the property passed in. The `entity` argument can be the entity name or an entity object.

## Returns

* This function returns the value casted to the right Java type

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entity | string | Yes |  | The entity name or entity object |
| propertyName | string | Yes |  | The property name |
| value | any | Yes |  | The property value |

## Examples

```javascript
baseService.autoCast( "User", "createdDate", arguments.myDate );
```

