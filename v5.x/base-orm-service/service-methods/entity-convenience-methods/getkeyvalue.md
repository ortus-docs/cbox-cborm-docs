# getKeyValue

Get the unique identifier value for the passed in entity, or `null` if the entity has no identifier yet (for example a new, unsaved entity)

## Returns

* This function returns _any_

## Arguments

| Key    | Type   | Required | Default | Description                       |
| ------ | ------ | -------- | ------- | --------------------------------- |
| entity | any    | Yes      | ---     | The entity to inspect for its id  |

## Examples

```javascript
var pkValue = ormService.getKeyValue( user );
```
