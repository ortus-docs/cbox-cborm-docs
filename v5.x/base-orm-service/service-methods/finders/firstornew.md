# firstOrNew

The first entity matching a criteria structure, or a new, unsaved entity populated with the criteria and the extra `properties`. The new entity is built with [new()](../creation-population/new.md), so it is autowired and `ORMPostNew` is announced.

## Returns

* This function returns the found or new entity

## Arguments

| Key        | Type   | Required | Default | Description                                                           |
| ---------- | ------ | -------- | ------- | --------------------------------------------------------------------- |
| entityName | string | Yes      | ---     | The entity to search                                                  |
| criteria   | struct | No       | `{}`    | The property values to search for, also used to populate a new entity |
| properties | struct | No       | `{}`    | Extra property values for a new entity; not used for the search       |

## Examples

```javascript
var tag = ormService.firstOrNew( "Tag", { name : rc.tag } );
if( !ormService.sessionContains( tag ) ){
    // a new, unsaved tag
}

var setting = ormService.firstOrNew( "Setting", { name : "theme" }, { value : "dark" } );
```
