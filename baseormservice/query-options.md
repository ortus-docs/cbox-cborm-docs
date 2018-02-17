#Query Options
If you pass a structure as the last argument to your dynamic finder/counter call, we will consider that by convention to be your query options.

```javascript
user = entityNew( "User" ).findByLastName( "Majano", { ignoreCase=true, timeout=20 } );

users = entityNew( "User" ).findAllByLastNameLike( "Ma%", { ignoreCase=false, max=20, offset=15 } );
```

The valid query options are:

* `ignorecase` : Ignores the case of sort order when you set it to true. Use this option only when you specify the sortorder parameter.
* `maxResults` : Specifies the maximum number of objects to be retrieved.
* `offset` : Specifies the start index of the resultset from where it has to start the retrieval.
* `cacheable` : Whether the result of this query is to be cached in the secondary cache. Default is false.
* `cachename` : Name of the cache in secondary cache.
* `timeout` : Specifies the timeout value (in seconds) for the query
