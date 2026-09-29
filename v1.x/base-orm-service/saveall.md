# saveAll

Saves an array of passed entities in specified order and transaction safe

## Returns

* This function returns _void_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entities | array | Yes | --- | The array of entities to persist |
| forceInsert | boolean | No | false |  |
| flush | boolean | No | false |  |
| transactional | boolean | No | true | Use ColdFusion transactions or not |

## Examples

```javascript
var user = ormService.new("User");
populateModel(user);
var user2 = ormService.new("User");
populateModel(user);

ormService.saveAll( [user1,user2] );
```

