# getDirtyPropertyNames

Get an array of property names that that have been changed from the original loaded session state.

## Returns

* An array of property names

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
