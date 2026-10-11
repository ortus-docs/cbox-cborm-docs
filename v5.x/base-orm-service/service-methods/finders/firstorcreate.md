# firstOrCreate

The first entity matching a criteria structure, or a new entity populated with the criteria and the extra `properties` and saved with [save()](../saving-entities/save.md).

## Returns

* This function returns the found or new, saved entity

## Arguments

| Key           | Type    | Required | Default           | Description                                                           |
| ------------- | ------- | -------- | ----------------- | --------------------------------------------------------------------- |
| entityName    | string  | Yes      | ---               | The entity to search                                                  |
| criteria      | struct  | No       | `{}`              | The property values to search for, also used to populate a new entity |
| properties    | struct  | No       | `{}`              | Extra property values for a new entity; not used for the search       |
| flush         | boolean | No       | false             | Flush the new entity right away                                       |
| transactional | boolean | No       | `useTransactions` | Wrap the save in a transaction                                        |

## Examples

```javascript
var tag = ormService.firstOrCreate( "Tag", { name : rc.tag } );

// flush so the new id is available
var setting = ormService.firstOrCreate( "Setting", { name : "theme" }, { value : "dark" }, true );
```
