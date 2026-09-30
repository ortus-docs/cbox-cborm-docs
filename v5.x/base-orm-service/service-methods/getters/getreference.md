# getReference

Get a reference (a lazy proxy) to an entity by id without loading it: no `SELECT` runs until you read a property other than the id. Great for setting associations when you only have the id. It wraps the `bx-orm` `entityGetReference()` function.

## Returns

* This function returns the entity reference

## Arguments

| Key        | Type   | Required | Default | Description     |
| ---------- | ------ | -------- | ------- | --------------- |
| entityName | string | Yes      | ---     | The entity name |
| id         | any    | Yes      | ---     | The id          |

## Examples

```javascript
var post = ormService.new( "Post", { title : rc.title } );
// no query for the author
post.setAuthor( ormService.getReference( "User", rc.authorID ) );
ormService.save( post );
```
