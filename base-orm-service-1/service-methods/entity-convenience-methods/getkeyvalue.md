# getKeyValue

Get the unique identifier value for the passed in entity, or `null` if the instance is not in session

## Returns

* This function returns _any_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entity | string | Yes | --- | The entity to inspect for it's id |

## Examples

```javascript
var pkValue = ormService.getKeyValue( "User" );
```

