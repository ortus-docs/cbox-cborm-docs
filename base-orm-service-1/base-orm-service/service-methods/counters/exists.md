# exists

Checks if the given `entityName` and `id` exists in the database, this method does not load the entity into session. A very useful approach to check for data existence.

## Returns

* This function returns _boolean_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | any | Yes | --- |  |
| id | any | Yes | --- |  |

## Examples

```javascript
if( ormService.exists("Account",123) ){
 // do something
}
```

