# list

List **all** of the instances of the passed in entity class name with or without any filtering of properties, no HQL needed. It delegates to the `bx-orm` `entityLoad()` function.

You can pass in several optional arguments like a struct of filtering criteria, a `sortOrder` string, `offset`, `max`, `ignorecase`, and `timeout`. Caching for the list is based on the _`useQueryCaching`_ class property and the _`cachename`_ property is based on the `queryCacheRegion` class property.

## Returns

* This function returns array if **asQuery = false**
* This function returns a query if **asQuery = true**
* This function returns a Java `Stream` read from the database as it is consumed if **asStream = true** (see [Java Streams](../../../advanced/java-streams.md))

## Arguments

| Key        | Type    | Required | Default          | Description                                                                   |
| ---------- | ------- | -------- | ---------------- | ----------------------------------------------------------------------------- |
| entityName | string  | Yes      | ---              | The entity to list                                                            |
| criteria   | struct  | No       |                  | A struct of filtering criteria for the listing                                |
| sortOrder  | string  | No       |                  | The sorting order of the listing                                              |
| offset     | numeric | No       | 0                | Pagination offset                                                             |
| max        | numeric | No       | 0                | Max records to return                                                         |
| timeout    | numeric | No       | 0                | Query timeout                                                                 |
| ignoreCase | boolean | No       | false            | Sort text properties case-insensitively (applies when you pass a `sortOrder`) |
| asQuery    | boolean | No       | `defaultAsQuery` | Return query or array of objects                                              |
| asStream   | boolean | No       | false            | Returns a Java `Stream` instead of an array                                   |
| options    | struct  | No       | `{}`             | More `bx-orm` `entityLoad()` options, e.g. `{ readOnly : true }`              |

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

// A Java stream, read from the database as it is consumed
activeNames = ormService.list( entityName="User", asStream = true )
    .filter( ( user ) => user.getIsActive() )
    .map( ( user ) => user.getFullName() )
    .toList();

// Read-only entities
users = ormService.list( entityName="User", options = { readOnly : true } );
```
