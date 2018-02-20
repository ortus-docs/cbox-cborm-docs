#populateFromJSON
Populate an entity with a JSON structure packet. Make sure the names of the properties match the keys in the structure.

###Returns

* This function returns *void*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| target | any | Yes | --- | The entity to populate |
| JSONString | string | Yes | --- | The JSON packet to use for population |
| scope | string | No |  | Use scope injection instead of setter injection, no need of setters, just tell us what scope to inject to |
| trustedSetter | Boolean | No | false | Do not check if the setter exists, just call it, great for usage with onMissingMethod() and virtual properties |
| include | string | No |  | A list of keys to ONLY include in the population |
| exclude | string | No |  | A list of keys to exclude from the population |

###Examples

```javascript
var user = ormService.populateFromJSON( ormService.new("User"), jsonString );
```