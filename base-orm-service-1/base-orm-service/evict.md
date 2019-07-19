# evict

Evict an entity from session, the id can be a string or structure for the primary key You can also pass in a collection name to evict from the collection

## Returns

* This function returns _void_

## Arguments

|  | Key | Type | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
|  | entityName | string | Yes | --- |
|  | collectionName | string | No | --- |
| id | any | No | --- |  |

## Examples

```javascript
ormService.evict(entityName="Account",account.getID());
ormService.evict(entityName="Account");
ormService.evict(entityName="Account", collectionName="MyAccounts");
```

