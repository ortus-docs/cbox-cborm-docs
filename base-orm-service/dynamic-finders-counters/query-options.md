# Query Options

If you pass a structure as the last argument to your dynamic finder/counter call, we will consider that by convention to be your query options. A struct is treated as options when it contains at least one of the option keys below.

```javascript
user = getInstance( "User" )
    .findByLastName( "Majano", { ignoreCase : true, timeout : 20 } );

users = getInstance( "User" )
    .findAllByLastNameLike( "Ma%", { maxResults : 20, offset : 15, sortBy : "lastName desc" } );
```

The valid query options are:

* `ignoreCase` : Ignores the case of sort order when you set it to true.
* `maxResults` : Specifies the maximum number of objects to be retrieved.
* `offset` : Specifies the start index of the resultset from where it has to start the retrieval.
* `cacheable` : Whether the result of this query is to be cached in the secondary cache. Default is false.
* `cacheName` : Name of the cache in secondary cache.
* `timeout` : Specifies the timeout value (in seconds) for the query
* `datasource` : The datasource to use, it defaults to the service's datasource
* `sortBy` : The HQL to sort the query by. Property names can be in any case: they are rewritten to their declared case.
* `asStream` : Want a [cbStreams](https://forgebox.io/view/cbStreams) stream back instead of the results, no problem!
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
    sortBy         : hql to sort by,
    asStream       : boolean (false)
}
```
