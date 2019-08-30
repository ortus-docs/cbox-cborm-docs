# save

Save an entity using hibernate transactions or not. You can optionally flush the session also.

## Returns

* This function returns _void_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entity | any | Yes | --- | The entity to save |
| forceInsert | boolean | No | false | Insert as new record whether it already exists or not |
| flush | boolean | No | false | Do a flush after saving the entity, false by default since we use transactions |
| transactional | boolean | No | true | Wrap the save in a ColdFusion transaction |

## Examples

```javascript
var user = ormService.new("User");
populateModel(user);
ormService.save(user);

// Save with immediate flush
var user = ormService.new(entityName="User", lastName="Majano");
ormService.save(entity=user, flush=true);
```

