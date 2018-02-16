Returns the key (id field) of a given entity, either simple or composite keys. If the key is a simple pk then it will return a string, if it is a composite key then it returns an array


###Returns

* This function returns *any*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- |  |

###Examples

```javascript
var pkField = ormService.getKey( "User" );
```

