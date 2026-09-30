# findByExample

Find all/single entities by example

## Returns

* This function returns _array_, or a single entity (or `null`) if **unique = true**

## Arguments

| Key     | Type    | Required | Default | Description                            |
| ------- | ------- | -------- | ------- | -------------------------------------- |
| example | any     | Yes      | ---     | The entity sample                      |
| unique  | boolean | false    | false   | Return a single entity instead of an array |

## Examples

```javascript
currentUser = ormService.findByExample( session.user, true );
```
