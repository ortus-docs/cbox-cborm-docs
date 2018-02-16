#findit
Finds and returns the first result for the given query or null if no entity was found. You can either use the query and params combination or send in an example entity to find.


###Returns

* This function returns *any*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| query | string | No | --- |  |
| params | any | No | [runtime expression] |  |
| example | any | No | --- |  |

###Examples

```javascript
// My First Post
ormService.findIt("from Post as p where p.author='Luis Majano'");
// With positional parameters
ormService.findIt("from Post as p where p.author=?", ["Luis Majano"]);
// with a named parameter (since 0.5)
ormService.findIt("from Post as p where p.author=:author and p.isActive=:active", { author="Luis Majano",active=true} );
// By Example
book = ormService.new(entityName="Book", author="Luis Majano");
ormService.findIt( example=book );
```