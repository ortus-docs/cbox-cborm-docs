# saveAll

Saves an array of passed entities in specified order in a single transaction. The `ORMPreSave` and `ORMPostSave` interception points are announced for each entity when `eventHandling` is enabled.

## Returns

* This function returns the service (`this`)

## Arguments

| Key           | Type    | Required | Default | Description                        |
| ------------- | ------- | -------- | ------- | ---------------------------------- |
| entities      | array   | Yes      | ---     | The array of entities to persist   |
| forceInsert   | boolean | No       | false   |                                    |
| flush         | boolean | No       | false   |                                    |
| transactional | boolean | No       | `useTransactions` | Wrap the call in a BoxLang `transaction{}` or not |

## Examples

```javascript
var user1 = populateModel( ormService.new( "User" ) );
var user2 = populateModel( ormService.new( "User" ) );

ormService.saveAll( [ user1, user2 ] );
```
