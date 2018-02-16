Save an entity using hibernate transactions or not. You can optionally flush the session also.

###Returns

* This function returns *void*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entity | any | Yes | --- |  |
| forceInsert | boolean | No | false |  |
| flush | boolean | No | false |  |
| transactional | boolean | No | true | Use ColdFusion transactions or not |

###Examples

```javascript
var user = ormService.new("User");
populateModel(user);
ormService.save(user);

// Save with immediate flush
var user = ormService.new(entityName="User", lastName="Majano");
ormService.save(entity=user, flush=true);
```