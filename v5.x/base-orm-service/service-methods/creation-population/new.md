# new

Get a new entity object by entity name. You can also pass in a structure called `properties` that will be used to populate the new entity with or you can use optional named parameters to call setters within the new entity to have shorthand population.

## Returns

* This function returns the newly created entity

## Arguments

| Key                  | Type    | Required | Default | Description                                                                         |
| -------------------- | ------- | -------- | ------- | ----------------------------------------------------------------------------------- |
| entityName           | any     | true     | ---     |                                                                                     |
| properties           | struct  | false    | {}      | A structure of name-value pairs to populate the new entity with                     |
| composeRelationships | boolean | false    | true    | Automatically attempt to compose relationships from the incoming properties memento |
| nullEmptyInclude     | string  | false    | ---     | A list of keys to NULL when empty                                                   |
| nullEmptyExclude     | string  | false    | ---     | A list of keys to NOT NULL when empty                                               |
| ignoreEmpty          | boolean | false    | false   | Ignore empty property values on populations                                         |
| include              | string  | false    | ---     | A list of keys to include in the population from the incoming properties memento    |
| exclude              | string  | false    | ---     | A list of keys to exclude in the population from the incoming properties memento    |

## Examples

```javascript
// return empty post entity
var post = ormService.new("Post");
var user = ormService.new(entityName="User",properties={firstName="Luis", lastName="Majano", age="32", awesome=true});
var user = ormService.new("User",{fname="Luis",lname="Majano",cool=false,awesome=true});
```
