# isDirty

Verifies if the passed in entity has dirty values or not since loaded from the database.

## Returns

* This function returns true if the entity values have been changed since loaded into the hibernate session, else false

## Arguments

| Key    | Type | Required | Default | Description       |
| ------ | ---- | -------- | ------- | ----------------- |
| entity | Any  | Yes      | ---     | The entity object |

## Examples

```javascript
if( ormService.isDirty( entity ) ){
    // The values have been modified
    log.debug( "entity values modified", ormService.getDirtyPropertyNames( entity ) );
}
```
