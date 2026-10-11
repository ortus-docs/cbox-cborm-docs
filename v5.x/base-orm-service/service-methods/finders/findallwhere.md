# findAllWhere

Find all entities according to criteria structure. Ex: `findAllWhere( "Category", { category : "Training" } )`, `findAllWhere( "User", { age : 40, retired : true } )`

## Returns

* This function returns _array_
* This function returns a Java `Stream` if **asStream = true** (see [Java Streams](../../../advanced/java-streams.md))

## Arguments

| Key        | Type    | Required | Default | Description                                                      |
| ---------- | ------- | -------- | ------- | ---------------------------------------------------------------- |
| entityName | string  | Yes      | ---     |                                                                  |
| criteria   | struct  | Yes      | ---     | A structure of criteria to filter on                             |
| sortOrder  | string  | false    | ---     | The sort ordering                                                |
| ignoreCase | boolean | false    | false   | Sort text properties case-insensitively                          |
| timeout    | numeric | false    | 0       | Query timeout in seconds                                         |
| asStream   | boolean | false    | false   | Returns a Java `Stream` instead of an array                      |
| options    | struct  | false    | `{}`    | More `bx-orm` `entityLoad()` options, e.g. `{ readOnly : true }` |

## Examples

```javascript
posts = ormService.findAllWhere(entityName="Post", criteria={author="Luis Majano"});
users = ormService.findAllWhere(entityName="User", criteria={isActive=true});
artists = ormService.findAllWhere(entityName="Artist", criteria={isActive=true, artist="Monet"});
```
