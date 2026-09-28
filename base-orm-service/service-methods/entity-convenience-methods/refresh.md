# refresh

Refresh the state of an entity or array of entities from the database

## Returns

* This function returns back the BaseORMService (`this`)

## Arguments

| Key    | Type | Required | Default | Description |
| ------ | ---- | -------- | ------- | ----------- |
| entity | any  | Yes      | ---     | The entity or array of entities to refresh |

## Examples

```javascript
var user = storage.getVar("UserSession");
ormService.refresh( user );

var users = [user1,user2,user3];
ormService.refresh( users );
```
