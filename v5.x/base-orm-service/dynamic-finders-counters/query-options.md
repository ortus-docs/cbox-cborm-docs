# Query Options

If you pass a structure as the last argument to your dynamic finder/counter call, we will consider that by convention to be your query options. A struct is treated as options when it contains at least one of the option keys below.

```javascript
user = getInstance( "User" )
    .findByLastName( "Majano", { ignoreCase : true, timeout : 20 } );

users = getInstance( "User" )
    .findAllByLastNameLike( "Ma%", { maxResults : 20, offset : 15, sortBy : "lastName desc" } );
```

The valid query options are:

* `ignoreCase` : Accepted for backwards compatibility and ignored.
* `maxResults` : Specifies the maximum number of objects to be retrieved.
* `offset` : Specifies the start index of the resultset from where it has to start the retrieval.
* `cacheable` : Whether the result of this query is to be cached in the secondary cache. Default is false.
* `cacheName` : Name of the cache in secondary cache.
* `timeout` : Specifies the timeout value (in seconds) for the query
* `datasource` : The datasource to use, it defaults to the service's datasource
* `sortBy` : The properties to sort by, each with an optional `asc` or `desc`: `"lastName desc, firstName"`. Property names and paths only; anything else raises `InvalidSortOrder`, so it can never inject HQL. Names can be in any case.
* `asStream` : Return a Java `Stream` read from the database as it is consumed. See [Java Streams](../../advanced/java-streams.md).
* `readOnly`, `uniqueFirst`, `fetchSize` : Passed to `bx-orm`'s `ormExecuteQuery()`.
* `autoCast` : Accepted for backwards compatibility and ignored: `bx-orm` converts values itself.

Here is a more descriptive key set with the types and defaults

```javascript
{
    ignoreCase     : boolean (false)
    maxResults     : numeric (0)
    offset         : numeric (0)
    cacheable      : boolean (false)
    cacheName      : string (default)
    timeout        : numeric (0)
    datasource     : string (defaults)
    sortBy         : properties to sort by ("lastName desc, firstName"),
    asStream       : boolean (false),
    readOnly       : boolean (false),
    uniqueFirst    : boolean (false),
    fetchSize      : numeric
}
```
