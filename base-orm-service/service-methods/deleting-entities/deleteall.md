# deleteAll

Deletes all the entity records found in the database in a transaction safe manner and returns the number of records removed. It runs a single HQL bulk `delete` statement.

{% hint style="danger" %}
No cascading will be done and no ORM events are fired, since the delete is done without loading the entities into the session but via a DML HQL statement. Use `delete()` if you need cascades.
{% endhint %}

## Returns

* This function returns _numeric_

## Arguments

| Key           | Type    | Required | Default       | Description             |
| ------------- | ------- | -------- | ------------- | ----------------------- |
| entityName    | string  | Yes      | ---           | The entity to purge     |
| flush         | boolean | No       | false         | Flush the session after deleting |
| transactional | boolean | No       | From Property | Use transactions or not |

## Examples

```javascript
ormService.deleteAll("Tags");
```
