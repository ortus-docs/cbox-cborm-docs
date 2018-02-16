Checks if the given entityName and id exists in the database

###Returns

* This function returns *boolean*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | any | Yes | --- |  |
| id | any | Yes | --- |  |

###Examples

```javascript
if( ormService.exists("Account",123) ){
 // do something
}
```