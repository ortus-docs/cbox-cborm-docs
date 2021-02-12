# findAll

Find all the entities for the specified query, named or positional arguments or by an example entity

## Returns

* This function returns _array_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| query | string | No | --- | The HQL Query to execute |
| params | any | No | \[runtime expression\] | Named or positional params |
| offset | numeric | No | 0 |  |
| max | numeric | No | 0 |  |
| timeout | numeric | No | 0 |  |
| ignoreCase | boolean | No | false |  |
| datasource | string | No |  |  |
| asStream | boolean | No | false |  |

## Examples

```javascript
// find all blog posts
ormService.findAll("Post");
// with a positional parameters
ormService.findAll("from Post as p where p.author=?",['Luis Majano']);
// 10 posts from Luis Majano staring from 5th post ordered by release date
ormService.findAll("from Post as p where p.author=? order by p.releaseDate",['Luis majano'],offset=5,max=10);

// Using paging params
var query = "from Post as p where p.author='Luis Majano' order by p.releaseDate" 
// first 20 posts 
ormService.findAll(query=query,max=20) 
// 20 posts starting from my 15th entry
ormService.findAll(query=query,max=20,offset=15);

// examples with named parameters
ormService.findAll("from Post as p where p.author=:author", {author='Luis Majano'})
ormService.findAll("from Post as p where p.author=:author", {author='Luis Majano'}, max=20, offset=5);

// query by example
user = ormService.new(entityName="User",firstName="Luis");
ormService.findAll( example=user );
```

