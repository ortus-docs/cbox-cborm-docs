Deletes all the entity records found in the database in a transaction safe matter and returns the number of records removed

###Returns

* This function returns *numeric*

###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- | The entity to purge |
| flush | boolean | No | false |  |
| transactional | boolean | No | From Property | Use transactions or not |

###Examples

```javascript
ormService.deleteAll("Tags");
```
