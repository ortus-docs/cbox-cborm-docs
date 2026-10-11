# exists

Checks if the given `entityName` and `id` exists in the database, this method does not load the entity into session. It runs an `EXISTS` query that stops at the first row. For an entity with a composite id, pass a struct of key values.

## Returns

* This function returns _boolean_

## Arguments

| Key        | Type | Required | Default | Description                                          |
| ---------- | ---- | -------- | ------- | ---------------------------------------------------- |
| entityName | any  | Yes      | ---     |                                                      |
| id         | any  | Yes      | ---     | The id, or a struct of key values for a composite id |

## Examples

```javascript
if( ormService.exists("Account",123) ){
 // do something
}

// composite id
if( ormService.exists( "EntryCategory", { entryId : 1, categoryId : 2 } ) ){
 // do something
}
```
