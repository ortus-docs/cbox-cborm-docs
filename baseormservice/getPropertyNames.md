#getPropertyNames
Returns the persisted Property Names of the entity in array format

###Returns

* This function returns *array*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- |  |

###Examples

```javascript
var properties = ormService.getPropertyNames("User");
```