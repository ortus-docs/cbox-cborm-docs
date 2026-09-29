# delete

Delete an entity inside a transaction. The entity argument can be a single entity or an array of entities. You can optionally flush the session after deleting.

{% hint style="info" %}
This method deletes the entities through the ORM session, so it respects cascading deletes and fires the ORM delete events.
{% endhint %}

## Returns

* This function returns the service (`this`) so you can chain calls

## Arguments

| Key           | type    | Required | Default       | Description                                                                 |
| ------------- | ------- | -------- | ------------- | --------------------------------------------------------------------------- |
| entity        | any     | Yes      | ---           | The entity or array of entities to delete                                   |
| flush         | boolean | No       | false         | Flush the session after deleting                                            |
| transactional | boolean | No       | From Property | Wrap the call in a BoxLang `transaction{}` (joins an active one if present) |

## Examples

```javascript
var post = ormService.get( "Post", 1 );
ormService.delete( post );

// Delete and flush immediately
ormService.delete( post, true );

// Delete several entities
ormService.delete( [ post1, post2 ] );
```
