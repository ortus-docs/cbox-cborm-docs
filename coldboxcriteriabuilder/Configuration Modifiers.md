The following methods alters the behavior of the executed query:

| Method | Description | Example |
| --- | --- | --- |
| timeout(numeric timeout) | Set a timeout for the underlying JDBC query in milliseconds. | *timeout( 5000 )* |
| readOnly(boolean readOnly) | Set the read-only/modifiable mode for entities and proxies loaded by this Criteria, defaults to readOnly=true | *readOnly(true)* |
| firstResult() | Specifies the offset for the results. A value of 0 will return all records up to the maximum specified. | *firstResult(11)* |
| maxResults(numeric maxResults) | Set a limit upon the number of objects to be retrieved. | *maxResults(25)* |
| fetchSize(numeric fetchSize) | Set's the fetch size of the underlying JDBC query | *fetchSize(50)* |
| cache(cache, cacheRegion= ) | Tells Hibernate whether to cache the query or not (if the query cache is enabled), and optionally choose a cache region | *cache(true), cache(true,'my.cache')* |
| cacheRegion(cacheRegion) | Tells Hibernate the cache region to store the query under | cacheRegion('my.cool.cache') |
| order(property,sortDir='asc',ignoreCase=false) | Specifies both the sort property (the first argument, the sort order (either 'asc' or 'desc'), and if it should ignore cases or not | *order('lastName','asc',false)* |

```javascript
c.timeout( 5000 )
c.readOnly(true)
c.firstResult(20).maxResults(50).fetchSize(10).cacheRegsion('my.awesome.region')
c.cache(true,'my.region')
c.order('lastName','desc',true);   
```