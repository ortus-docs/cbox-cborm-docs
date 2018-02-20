#delete
Delete an entity using safe transactions. The entity argument can be a single entity or an array of entities. You can optionally flush the session also after committing.

###Returns

* This function returns *void*

###Arguments

| Key | type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entity | any | Yes | --- |  |
| flush | boolean | No | false |  |
| transactional | boolean | No | From Property | Use Transactions or not |

###Examples


```javascript
var post = ormService.get(1);
ormService.delete( post );

// Delete a flush immediately
ormService.delete( post, true );
```



