# populateFromXML

Populate an entity with an XML packet. Make sure the names of the elements match the keys in the structure.

## Returns

* This function returns the populated object

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| target | any | Yes | --- | The entity to populate |
| xml | any | Yes | --- | The xml string or xml document object to populate with |
| root | string | false |  | The xml root node to start the population with, by default it uses the XMLRoot. |
| scope | string | No |  | Use scope injection instead of setter injection, no need of setters, just tell us what scope to inject to |
| trustedSetter | Boolean | No | false | Do not check if the setter exists, just call it, great for usage with onMissingMethod\(\) and virtual properties |
| include | string | No |  | A list of keys to ONLY include in the population |
| exclude | string | No |  | A list of keys to exclude from the population |
| nullEmptyInclude | string | No |  | A list of keys to NULL when empty, specifically for ORM population. You can also specify "\*" for all fields |
| nullEmptyExclude | string | No |  | A list of keys to NOT NULL when empty, specifically for ORM population. You can also specify "\*" for all fields |
| composeRelationships | boolean | No | true | When true, will automatically attempt to compose relationships from memento |

## Examples

```javascript
var user = ormService.populateFromXML( ormService.new("User"), xml, "User");
```

