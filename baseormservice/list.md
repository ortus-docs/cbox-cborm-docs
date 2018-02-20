#list
List all of the instances of the passed in entity class name. You can pass in several optional arguments like a struct of filtering criteria, a sortOrder string, offset, max, ignorecase, and timeout. Caching for the list is based on the *useQueryCaching* class property and the *cachename* property is based on the queryCacheRegion class property.


### Returns

* This function returns *array*


### Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- |  |
| criteria | struct | No | [runtime expression] |  |
| sortOrder | string | No |  |  |
| offset | numeric | No | 0 |  |
| max | numeric | No | 0 |  |
| timeout | numeric | No | 0 |  |
| ignoreCase | boolean | No | false |  |
| asQuery | boolean | No | true |  |

### Examples

```javascript
users = ormService.list(entityName="User",max=20,offset=10,asQuery=false);
users = ormService.list(entityName="Art",timeout=10);
users = ormService.list("User",{isActive=false},"lastName, firstName");
users = ormService.list("Comment",{postID=rc.postID},"createdDate desc");
```