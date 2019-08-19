# idCast

This method allows you to cast the identifier value to the appropriate type in Java. The `entity` argument can be the entity name or an entity object. Please note that this is ONLY used for identifier casting not for any property!

## Returns

* This function returns the value casted to the right Java type

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entity | string | Yes |  | The entity name or entity object |
| id | any | Yes |  | The value to cast |

## Examples

```javascript
baseService.idCast( "User", "123" );
```

