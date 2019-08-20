# deleteAll

Deletes all the entity records found in the database in a transaction safe matter and returns the number of records removed

{% hint style="info" %}
This method will respect cascading deletes if any
{% endhint %}

## Returns

* This function returns _numeric_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- | The entity to purge |
| flush | boolean | No | false |  |
| transactional | boolean | No | From Property | Use transactions or not |

## Examples

```javascript
ormService.deleteAll("Tags");
```

