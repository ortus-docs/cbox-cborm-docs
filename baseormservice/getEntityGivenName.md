Returns the entity name from a given entity object via session lookup or if new object via metadata lookup


###Returns

* This function returns *string*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | any | Yes | --- |  |

###Examples

```javascript
var name = ORMService.getEntityGivenName( entity );
```