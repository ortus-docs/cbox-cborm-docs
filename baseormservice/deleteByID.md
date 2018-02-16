Delete using an entity name and an incoming id, you can also flush the session if needed. The ID can be a single ID or an array of ID's to batch delete using hibernate DLM style deletes. The function also returns the number of records deleted.

###Returns
* This function returns *numeric*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- | The name of the entity to delte |
| id | any | Yes | --- | A single ID or array of IDs |
| flush | boolean | No |false  |  |
| transactional | boolean | No | From Property | Use transactions not |

###Examples


```javascript
// just delete
count = ormService.deleteByID("User",1);

// delete and flush
count = ormService.deleteByID("User",4,true);

// Delete several records, or at least try
count = ormService.deleteByID("User",[1,2,3,4]);
```
