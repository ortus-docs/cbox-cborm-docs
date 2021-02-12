# list

List **all** of the instances of the passed in entity class name with or without any filtering of properties, no HQL needed. 

You can pass in several optional arguments like a struct of filtering criteria, a `sortOrder` string, `offset`, `max`, `ignorecase`, and `timeout`. Caching for the list is based on the _`useQueryCaching`_ class property and the _`cachename`_ property is based on the `queryCacheRegion` class property.

## Returns

* This function returns array if **asQuery = false**
* This function returns a query if **asQuery = true**
* This function returns a stream if **asStream = true**

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- | The entity to list |
| criteria | struct | No |  | A struct of filtering criteria for the listing |
| sortOrder | string | No |  | The sorting order of the listing |
| offset | numeric | No | 0 | Pagination offset |
| max | numeric | No | 0 | Max records to return |
| timeout | numeric | No | 0 | Query timeout |
| ignoreCase | boolean | No | false | Case insensitive or case sensitive searches, we default to case sensitive filtering.  |
| asQuery | boolean | No | false | Return query or array of objects |
| asStream | boolean | No | false | Returns the result as a Java Stream using [cbStreams](https://forgebox.io/view/cbstreams) |



## Examples

```javascript
aUsers = ormService.list( 
    entityName="User", 
    max=20, 
    offset=10
);

users = ormService.list( entityName="Art", timeout=10 );

users = ormService.list( "User", {isActive=false}, "lastName,firstName" );

users = ormService.list( "Comment", {postID=rc.postID}, "createdDate desc" );

qUsers = ormService.list( entityName="User", asQuery = true );
```

