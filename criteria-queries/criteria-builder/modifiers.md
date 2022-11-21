# Modifiers

## Query Modifiers

The following methods alters the behavior of the executed query, some can be a life saver, so check them all out.

| Method                                           | Description                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cache(boolean cache=true, cacheRegion)`         | Tells Hibernate whether to cache the query or not (if the query cache is enabled), and optionally choose a cache region                                                                                                                                                                                                                         |
| `cacheRegion(cacheRegion)`                       | Tells Hibernate the cache region to store the query under                                                                                                                                                                                                                                                                                       |
| `comment(comment)`                               | Add a comment to the generated SQL.                                                                                                                                                                                                                                                                                                             |
| `fetchSize(numeric fetchSize)`                   | Set's the fetch size of the underlying JDBC query                                                                                                                                                                                                                                                                                               |
| `firstResult(numeric firstResult)`               | Specifies the offset for the results. A value of 0 will return all records up to the maximum specified.                                                                                                                                                                                                                                         |
| `maxResults(numeric maxResults)`                 | Set a limit upon the number of objects to be retrieved.                                                                                                                                                                                                                                                                                         |
| `order(property,sortDir='asc',ignoreCase=false)` | Specifies both the sort property (the first argument, the sort order (either 'asc' or 'desc'), and if it should ignore cases or not                                                                                                                                                                                                             |
| `peek( target )`                                 | Peek into the criteria build process with your own closure that accepts the criteria itself.                                                                                                                                                                                                                                                    |
| `queryHint(hint)`                                | Add a DB query hint to the SQL. These differ from JPA's QueryHint, which is specific to the JPA implementation and ignores DB vendor-specific hints. Instead, these are intended solely for the vendor-specific hints, such as Oracle's optimizers. Multiple query hints are supported; the Dialect will determine concatenation and placement. |
| `readOnly(boolean readOnly)`                     | Set the read-only/modifiable mode for entities and proxies loaded by this Criteria, defaults to `readOnly=true`                                                                                                                                                                                                                                 |
| `timeout(numeric timeout)`                       | Set a timeout for the underlying JDBC query in milliseconds.                                                                                                                                                                                                                                                                                    |
| `when( test, target )`                           | A nice functional method to allow you to pass a boolean evaulation and if true, the target closure will be executed for you, which will pass in the criteria object to it.                                                                                                                                                                      |

```javascript
c.timeout( 5000 )
c.readOnly(true)
c.firstResult(20).maxResults(50).fetchSize(10).cacheRegsion('my.awesome.region')
c.cache(true,'my.region')
c.order('lastName','desc',true);

newCriteria()
	 .eq( "this", value )
	 .peek( function(criteria){
	 	systemOutput( "CurrentSQL: #criteria.getSQLLog()#" )
	 })
	 .when( !isNull( arguments.published ), function( c ){
		c.eq( "isPublished", published )
	 })
	 .list();
```

## Result Modifiers

You can also tell Hibernate to transform the results to other formats for you once you retrieve them.

* `asDistinct()` - Applies a result transformer of DISTINCT\_ROOT\_ENTITY
* `asStruct()` - Applies a result transformer of ALIAS\_TO\_ENTITY\_MAP so you get an array of structs instead of array of objects
* `asStream()` - Get the results as a CBstream
