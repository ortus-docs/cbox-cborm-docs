# findWhere

Find one entity \(or null if not found\) according to a criteria structure ex: findWhere\(entityName="Category", {category="Training"}\), findWhere\(entityName="Users",{age=40,retired=false}\);

## Returns

* This function returns _any_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- |  |
| criteria | struct | Yes | --- | A structure of criteria to filter on |

## Examples

```javascript
// Find a category according to the named value pairs I pass into this method
var category = ormService.findWhere(entityName="Category", criteria={isActive=true, label="Training"});
var user = ormService.findWhere(entityName="User", criteria={isActive=true, username=rc.username,password=rc.password});
```

