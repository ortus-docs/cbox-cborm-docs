# getKey

Returns the key (id field) of a given entity, either simple or composite keys.

* If the key is a simple pk then it will return a string, if it is a composite key then it returns an array.
* If the key cannot be identified then a blank string is returned.

## Returns

* This function returns _any_

## Arguments

| Key    | Type   | Required | Default | Description                      |
| ------ | ------ | -------- | ------- | -------------------------------- |
| entity | string | Yes      | ---     | The entity name or entity object |

## Examples

```javascript
var pkField = ormService.getKey( "User" );
```
