# Configuration Modifiers

The following methods alters the behavior of the executed query, some can be a life saver, so check them all out.

| Method | Description | Example |
| :--- | :--- | :--- |
| `timeout(numeric timeout)` | Set a timeout for the underlying JDBC query in milliseconds. | _timeout\( 5000 \)_ |
| `readOnly(boolean readOnly)` | Set the read-only/modifiable mode for entities and proxies loaded by this Criteria, defaults to readOnly=true | _readOnly\(true\)_ |
| `firstResult()` | Specifies the offset for the results. A value of 0 will return all records up to the maximum specified. | _firstResult\(11\)_ |
| `maxResults(numeric maxResults)` | Set a limit upon the number of objects to be retrieved. | _maxResults\(25\)_ |
| `fetchSize(numeric fetchSize)` | Set's the fetch size of the underlying JDBC query | _fetchSize\(50\)_ |
| `cache(cache, cacheRegion= )` | Tells Hibernate whether to cache the query or not \(if the query cache is enabled\), and optionally choose a cache region | _cache\(true\), cache\(true,'my.cache'\)_ |
| `cacheRegion(cacheRegion)` | Tells Hibernate the cache region to store the query under | cacheRegion\('my.cool.cache'\) |
| `order(property,sortDir='asc',ignoreCase=false)` | Specifies both the sort property \(the first argument, the sort order \(either 'asc' or 'desc'\), and if it should ignore cases or not | _order\('lastName','asc',false\)_ |

```javascript
c.timeout( 5000 )
c.readOnly(true)
c.firstResult(20).maxResults(50).fetchSize(10).cacheRegsion('my.awesome.region')
c.cache(true,'my.region')
c.order('lastName','desc',true);
```

