# new

Get a new entity object by entity name. You can also pass in a structure called properties that will be used to populate the new entity with or you can use optional named parameters to call setters within the new entity to have shorthand population.

## Returns

* This function returns _any_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | any | true | --- |  |
| properties | struct | false | {} | A structure of name-value pairs to populate the new entity with |

## Examples

```javascript
// return empty post entity
var post = ormService.new("Post");
var user = ormService.new(entityName="User",properties={firstName="Luis", lastName="Majano", age="32", awesome=true});
var user = ormService.new("User",{fname="Luis",lname="Majano",cool=false,awesome=true});
```

