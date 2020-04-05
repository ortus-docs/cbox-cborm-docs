# populateFromQuery

Populate an entity with a query object. Make sure the names of the columns match the keys in the object.

## Returns

* This function returns the populated object

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| target | any | Yes | --- | The entity to populate |
| qry | query | Yes | --- | The query to populate with |
| rowNumber | numeric | false | 1 | The row to use to populate with. |
| scope | string | No | --- | Use scope injection instead of setter injection, no need of setters, just tell us what scope to inject to |
| trustedSetter | Boolean | No | false | Do not check if the setter exists, just call it, great for usage with onMissingMethod\(\) and virtual properties |
| include | string | No | --- | A list of columns to ONLY include in the population |
| exclude | string | No | --- | A list of columns to exclude from the population |
| nullEmptyInclude | string | No |  | A list of keys to NULL when empty, specifically for ORM population. You can also specify "\*" for all fields |
| nullEmptyExclude | string | No |  | A list of keys to NOT NULL when empty, specifically for ORM population. You can also specify "\*" for all fields |
| composeRelationships | boolean | No | true | When true, will automatically attempt to compose relationships from memento |

## Examples

```javascript
var user = ormService.populateFromQuery( ormService.new( "User" ), ormService.list( "User", { id=4 } ) );
```

