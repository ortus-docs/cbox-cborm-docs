# deleteByID

Delete using an entity name and an incoming id, you can also flush the session if needed. The ID can be a single ID, a list of IDs or an array of IDs to batch delete using a DML style HQL delete. For an entity with a composite id, pass a struct of key values, or an array of them. Pending changes in the session are flushed before the delete runs, and a `softDelete` entity is marked deleted instead. The function also returns the number of records deleted.

{% hint style="danger" %}
No cascading will be done since the delete is done without loading the entity into session but via a DML HQL statement
{% endhint %}

## Returns

* This function returns _numeric_

## Arguments

| Key           | Type    | Required | Default       | Description                                                                        |
| ------------- | ------- | -------- | ------------- | ---------------------------------------------------------------------------------- |
| entityName    | string  | Yes      | ---           | The name of the entity to delete                                                   |
| id            | any     | Yes      | ---           | A single ID, list or array of IDs; a struct or array of structs for a composite id |
| flush         | boolean | No       | false         |                                                                                    |
| transactional | boolean | No       | From Property | Use transactions or not                                                            |

## Examples

```javascript
// just delete
count = ormService.deleteByID("User",1);

// delete and flush
count = ormService.deleteByID("User",4,true);

// Delete several records, or at least try
count = ormService.deleteByID("User",[1,2,3,4]);

// composite id
count = ormService.deleteByID( "EntryCategory", { entryId : 1, categoryId : 2 } );
```
