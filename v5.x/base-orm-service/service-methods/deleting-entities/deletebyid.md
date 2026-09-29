# deleteByID

Delete using an entity name and an incoming id, you can also flush the session if needed. The ID can be a single ID, a list of IDs or an array of IDs to batch delete using a DML style HQL delete. The function also returns the number of records deleted.

{% hint style="danger" %}
No cascading will be done since the delete is done without loading the entity into session but via a DML HQL statement
{% endhint %}

## Returns

* This function returns _numeric_

## Arguments

| Key           | Type    | Required | Default       | Description                     |
| ------------- | ------- | -------- | ------------- | ------------------------------- |
| entityName    | string  | Yes      | ---           | The name of the entity to delete |
| id            | any     | Yes      | ---           | A single ID, list or array of IDs |
| flush         | boolean | No       | false         |                                 |
| transactional | boolean | No       | From Property | Use transactions or not         |

## Examples

```javascript
// just delete
count = ormService.deleteByID("User",1);

// delete and flush
count = ormService.deleteByID("User",4,true);

// Delete several records, or at least try
count = ormService.deleteByID("User",[1,2,3,4]);
```
