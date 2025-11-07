# deleteByQuery

Delete by using an HQL query and iterating via the results. It is not performing a delete query but  is a select query that should retrieve objects to remove

## Returns

* This function returns _void_

## Arguments

| Key           | Type    | Required | Default       | description                                         |
| ------------- | ------- | -------- | ------------- | --------------------------------------------------- |
| query         | string  | Yes      | ---           |                                                     |
| params        | any     | No       | ---           |                                                     |
| max           | numeric | No       | 0             |                                                     |
| offfset       | numeric | No       | 0             |                                                     |
| flush         | boolean | No       | false         |                                                     |
| transactional | boolean | No       | From Property | Use transactions or not                             |
| datasource    | string  | false    |               | The datasource to use or use the default datasource |

## Examples

```javascript
// delete all blog posts
ormService.deleteByQuery("from Post");
// delete query with positional parameters
ormService.deleteByQuery("from Post as b where b.author=? and b.isActive = :active",['Luis Majano',false]);

// Use query options
var query = "from User as u where u.isActive=false order by u.creationDate desc"; 
// first 20 stale inactive users 
ormService.deleteByQuery(query=query,max=20); 
// 20 posts starting from my 15th entry
ormService.deleteByQuery(query=query,max=20,offset=15,flush=true);

// examples with named parameters
ormService.deleteByQuery("from Post as p where p.author=:author", {author='Luis Majano'})
```
