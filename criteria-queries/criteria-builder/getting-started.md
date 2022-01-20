# Getting Started

A criteria builder object can be requested from our Base ORM services or a virtual service or an ActiveEntity, which will bind itself automatically to the requested entity, by calling on the `newCriteria()` method. The corresponding class is: `cborm.models.CriteriaBuilder`

## Criteria Object - `newCriteria()`

The arguments for the `newCriteria()` method are:

| Argument           | Type    | Required | Default                 | Description                                                                           |
| ------------------ | ------- | -------- | ----------------------- | ------------------------------------------------------------------------------------- |
| `entityName`       | string  | true     | ---                     | The name of the entity to bind this criteria builder with, the initial pivot.         |
| `useQueryCaching`  | boolean | false    | false                   | To allow for query caching of _list()_ operations                                     |
| `queryCacheRegion` | string  | false    | `criteria.{entityName}` | The name of the cache region to use                                                   |
| `datasource`       | string  | false    | System Default          | The datasource to bind the criteria query on, defaults to the one in this ORM service |

{% hint style="warning" %}
If you call `newCriteria()` from a virtual service layer or Active Entity, then you don't pass the `entityName` argument as it roots itself automatically.
{% endhint %}

```javascript
// orm service
ormService.newCriteria( "User" );

// virtual service
productService.newCriteria();

// active entity
getInstance( "User" ).newCriteria();
```

## Restrictions

This criteria object will then be used to add **restrictions** to build up the exact query you want. Restrictions are basically your _where_ statements in SQL and they build on each other via ANDs by default.  For example, only retrieve products with a price over $30 or give me only active users. &#x20;

We provide you with tons of available [restrictions](restrictions/) and if none of those match what you need, you can even use a-la-carte SQL restrictions, in which you can just use SQL even with parameters.  You can also do OR statements or embedded ANDs, etc. &#x20;

```javascript
productService
    .newCriteria()
    .ge( "price", 30 )
    .count();
    
userService
    .newCriteria()
    .isTrue( "isActive" )
    .notIsNull( "lastLogin" )
    .orderBy( "lastLogin desc" )
    .firstResult()
    .get();


// A-la-carte SQL restrictions 
var userStream = userService
    .newCriteria()
    .sql( "userName = ? and firstName like ? and lastLogin >= ?", [
    	{ value : "joe", type : "string" },
    	{ value : "%joe%", type : "string" }
        { value : incomingDate, type : "timestamp" }
    ] )
    .list( asStream = true );
    

var c = newCriteria();
var results = c.like("firstName","Lui%") // restriction
     .maxResults( 50 ) // modifier
     .order("balance","desc") // modifier
     // AND restrictions
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     // Retrieve a list
     .list();
```

{% hint style="success" %}
**Tip**: Every restriction can also be negated by using the `not` prefix before each method: `notEq(), notIn(), notIsNull()`
{% endhint %}

## Associations

You can also use your restrictions on the associated entity data. This is achieved via the [association](associations.md) methods section.

## Query Modifiers

You can also add [modifiers](modifiers.md) for the execution of the query.  This can be sorting, timeouts, join types and so much more.

* `cache()` - Enable caching of this query result, provided query caching is enabled for the underlying session factory.
* `cacheRegion()` - Set the name of the cache region to use for query result caching.
* `comment()` - Add a comment to the generated SQL.
* `fetchSize()` - Set a fetch size for the underlying JDBC query.
* `firstResult()` - Set the first result to be retrieved or the offset integer
* `maxResults()` - Set a limit upon the number of objects to be retrieved.
* `order()` - Add an ordering to the result set, you can add as many as you like
* `queryHint()` - Add a DB query hint to the SQL. These differ from JPA's QueryHint, which is specific to the JPA implementation and ignores DB vendor-specific hints. Instead, these are intended solely for the vendor-specific hints, such as Oracle's optimizers. Multiple query hints are supported; the Dialect will determine concatenation and placement.
* `readOnly()` - Set the read-only/modifiable mode for entities and proxies loaded by this Criteria, defaults to readOnly=true
* `timeout()` - Set a timeout for the underlying JDBC query.

```javascript
var results = c.like("firstName","Lui%") // restriction
     .firstResult( 25 ) // modifier
     .maxResults( 50 ) // modifier
     .order( "balance", "desc" ) // modifier
     .timeout( 
     // AND restrictions
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     // Retrieve a list
     .list();
```

## Result Modifiers

You can also tell Hibernate to transform the results to other formats for you once you retrieve them.

* `asDistinct()` - Applies a result transformer of DISTINCT\_ROOT\_ENTITY
* `asStruct()` - Applies a result transformer of ALIAS\_TO\_ENTITY\_MAP so you get an array of structs instead of array of objects
* `asStream()` - Get the results as a CBstream

## Getting Results

Now that the criteria builder object has all the restrictions and modifiers attached when can execute the SQL.  Please note that you can store a criteria builder object if you wanted to. It is lazy evaluated, it just represents your SQL.  It will only execute when you need it to execute via the following finalizer methods:

* `list()` - Execute the criteria queries you have defined and return the results as an array of objects
* `get( [properties] )` - Convenience method to return a single instance that matches the built up criterias query, or **null** if the query returns **no** results.&#x20;
* `getOrFail( [properties] )` - Convenience method to return a single instance that matches the built up criterias query, or throws an exception if the query returns no results
* `count()` - Get the record count using hibernate projections for the given criterias

```javascript
productService
    .newCriteria()
    .ge( "price", 30 )
    .count();
    
userService
    .newCriteria()
    .isTrue( "isActive" )
    .notIsNull( "lastLogin" )
    .orderBy( "lastLogin desc" )
    .firstResult()
    .get();
```

## Logging

There are several methods available to you in the criteria objects to give you the actual SQL or HQL to execute, even with bindings.  These are a true life-saver.

* `logSQL( label )` - Allows for one-off sql logging at any point in the process of building up CriteriaBuilder; will log the SQL state at the time of the call
* `getSQL( returnExecutableSql = false, formatSql )` - Returns the SQL string that will be prepared for the criteria object at the time of request. If you set returnExecutableSql to true , the SQL returned will include the parameters populated.
* `getPositionalSQLParameters()` - Returns a formatted array of parameter value and types
* `getSqlLog()` - Retrieves the SQL Log
* `startSqlLog()` - Triggers CriteriaBuilder to start internally logging the state of SQL at each iterative build
* `stopSqlLog()` - Stop the internal logging.
* `logSql()` - Allows for one-off sql logging at any point in the process of building up CriteriaBuilder; will log the SQL state at the time of the call
* `canLogSql()` - Returns whether or not CriteriaBuilder is currently configured to log SQL
* `peek( function/closure )` - Used to peek into the criteria builder process. You pass in a closure/lambda that receives the criteria. You can then use it to peek into the sql or more.

```javascript
var sql = userService
    .newCriteria()
    .sql( "userName = ? and firstName like ? and lastLogin >= ?", [
    	{ value : "joe", type : "string" },
    	{ value : "%joe%", type : "string" }
        { value : incomingDate, type : "timestamp" }
    ] )
    .getSqlLog();
    
// You can also use peek()
var results = userService
    .newCriteria()
    .sql( "userName = ? and firstName like ? and lastLogin >= ?", [
    	{ value : "joe", type : "string" },
    	{ value : "%joe%", type : "string" }
        { value : incomingDate, type : "timestamp" }
    ] )
    .peek( function( c ){
        log.debug( "sql log: #c.getSqlLog()#" );
    });
    .list();
```
