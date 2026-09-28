# getEntityGivenName

Returns the entity name from a given entity object. It delegates to the `bx-orm` `entityGetName()` function.

## Returns

* This function returns _string_

## Arguments

| Key    | Type | Required | Default | Description |
| ------ | ---- | -------- | ------- | ----------- |
| entity | any  | Yes      | ---     |             |

## Examples

```javascript
var name = ORMService.getEntityGivenName( entity );
```
