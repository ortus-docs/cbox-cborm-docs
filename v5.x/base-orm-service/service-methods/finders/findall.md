# findAll

Find all the entities for the specified HQL query and its named or positional parameters.

## Returns

* This function returns _array_
* This function returns a Java `Stream` if **asStream = true** (see [Java Streams](../../../advanced/java-streams.md))

## Arguments

| Key        | Type    | Required | Default            | Description                                 |
| ---------- | ------- | -------- | ------------------ | ------------------------------------------- |
| query      | string  | No       | ---                | The HQL Query to execute                    |
| params     | any     | No       | `{}`               | Named (struct) or positional (array) params |
| offset     | numeric | No       | 0                  | Pagination offset                           |
| max        | numeric | No       | 0                  | Max records to return                       |
| timeout    | numeric | No       | 0                  | Query timeout                               |
| ignoreCase | boolean | No       | false              | Ignored, kept for compatibility             |
| datasource | string  | No       | Service datasource | The datasource to use                       |
| asStream   | boolean | No       | false              | Returns a Java `Stream` instead of an array |
| options    | struct  | No       | `{}`               | More `bx-orm` `ormExecuteQuery()` options   |

{% hint style="info" %}
Positional parameters can be written as plain `?` or numbered `?1`, `?2` (JPA style). Named parameters use `:name`. Entity and property names in the HQL can be in any case: `bx-orm` resolves them.
{% endhint %}

## Examples

```javascript
// find all blog posts
ormService.findAll( "from Post" );
// with a positional parameters
ormService.findAll( "from Post as p where p.author=?", [ 'Luis Majano' ] );
// 10 posts from Luis Majano starting from 5th post ordered by release date
ormService.findAll( "from Post as p where p.author=?1 order by p.releaseDate", [ 'Luis Majano' ], offset=5, max=10 );

// Using paging params
var query = "from Post as p where p.author='Luis Majano' order by p.releaseDate"
// first 20 posts
ormService.findAll( query=query, max=20 )
// 20 posts starting from my 15th entry
ormService.findAll( query=query, max=20, offset=15 );

// examples with named parameters
ormService.findAll( "from Post as p where p.author=:author", { author='Luis Majano' } )
ormService.findAll( "from Post as p where p.author=:author", { author='Luis Majano' }, max=20, offset=5 );
```

If you want to query by example, use [`findByExample()`](findbyexample.md).
