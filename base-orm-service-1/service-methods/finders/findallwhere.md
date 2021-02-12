# findAllWhere

Find all entities according to criteria structure. Ex: findAllWhere\(entityName="Category", {category="Training"}\), findAllWhere\(entityName="Users", {age=40,retired=true}\);

## Returns

* This function returns _array_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- |  |
| criteria | struct | Yes | --- | A structure of criteria to filter on |
| sortOrder | string | false | --- | The sort ordering |
| ignoreCase | boolean | false | false |  |
| timeout | numeric | false | 0 |  |
| asStream | boolean | false | false |  |

## Examples

```javascript
posts = ormService.findAllWhere(entityName="Post", criteria={author="Luis Majano"});
users = ormService.findAllWhere(entityName="User", criteria={isActive=true});
artists = ormService.findAllWhere(entityName="Artist", criteria={isActive=true, artist="Monet"});
```

