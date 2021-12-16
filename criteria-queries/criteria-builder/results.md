# Results

Once you have concatenated criterias together, you can execute the query via the execution methods. Please remember that these methods return the results, so they must be executed last.

So the idea is the you request a query, add all kinds of restrictions and modifiers and then you request the results from it.

| Method                                                                                                                                                                                                                                                                       | Description                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `count( propertyName="" )`                                                                                                                                                                                                                                                   | Note, count() can't be called on a criteria after list() has been executed.                                                                      |
| `get( [properties=""] )`                                                                                                                                                                                                                                                     | Get a single entity.  If you pass in the `properties` then you will get a struct of those properties.                                            |
| `getOrFail( [properties=""] )`                                                                                                                                                                                                                                               | Convenience method to return a single instance that matches the built up criterias query, or throws an exception if the query returns no results |
| <p><code>list(</code></p><p>  <code>max,</code></p><p>  <code>offset,</code></p><p>  <code>timeout,</code></p><p>  <code>sortOrder,</code></p><p>  <code>ignoreCase,</code> </p><p>  <code>asQuery=false</code></p><p>  <code>asStream=false</code></p><p><code>)</code></p> | Execute the criterias and give you the results.                                                                                                  |

### count( propertyName = "' )

Get the record count using hibernate projections for the given criterias.  You can optionally pass in the name of the property to do the count on, else it doesn `*` by default.

```java
service
    .newCriteria()
    .joinTo( "categories", "categories" )
    .isEq( "categories.categoryID", getCategoryID() )
    .isTrue( "isPublished" )
    .isLE( "publishedDate", now() )
    .isEq( "passwordProtection", "" )
    .$or(
        service.getRestrictions().isNull( "expireDate" ),
        service.getRestrictions().isGT( "expireDate", now() )
    )
    .cache( true )
    .count( "contentID" );
```

### get( properties="" )

Convenience method to return a single instance that matches the built up criterias, or `null` if the query returns no results.  It can also throw the following exception: **NonUniqueResultException** - if there is more than one matching result.  It can also take in a property list in the `properties` argument so instead of giving you a full ORM entity object, it will give you a struct of those properties.

```java
// Get an entity
var oTargetCategory = newCriteria()
    .isEq( "slug", arguments.category.getSlug() )
    .joinTo( "site", "site" )
    .isEq( "site.slug", arguments.site.getSlug() )
    .get();
    
// Get a struct of only the properties we need
var data = newCriteria()
    .isEq( "slug", arguments.category.getSlug() )
    .joinTo( "site", "site" )
    .isEq( "site.slug", arguments.site.getSlug() )
    .get( "id,slug" );	
```

### getOrFail( properties="" )

Convenience method to return a single instance that matches the built up criterias, or throw an exception (`EntityNotFound`) if the query returns no results.  It can also throw the following exception: **NonUniqueResultException** - if there is more than one matching result.  It can also take in a property list in the `properties` argument so instead of giving you a full ORM entity object, it will give you a struct of those properties.

```javascript
// Get an entity
var oTargetCategory = newCriteria()
    .isEq( "slug", arguments.category.getSlug() )
    .joinTo( "site", "site" )
    .isEq( "site.slug", arguments.site.getSlug() )
    .getOrFail();
    
// Get a struct of only the properties we need
var data = newCriteria()
    .isEq( "slug", arguments.category.getSlug() )
    .joinTo( "site", "site" )
    .isEq( "site.slug", arguments.site.getSlug() )
    .getOrFail( "id,slug" );
```

### list( ... )

Execute the criteria queries you have defined and return the results, you can pass optional parameters to manipulate the way the results are sent back to you.  Let's look at the method signature for this awesome method:

```javascript
/**
 * Execute the criteria queries you have defined and return the results, you can pass optional parameters or define them via our methods
 *
 * @offset     The pagination offset, defaults to 0
 * @max        The max number of records to get, defaults to all
 * @timeout    The query timeout
 * @sortOrder  The sorting order
 * @ignoreCase For the sorting and SQL
 * @asQuery    Return a query or array of data (objects/struct), defaults to arrays
 * @asStream   Return a cbStream of array data, defaults to the `asStream` property
 */
any function list(
	numeric offset     = 0,
	numeric max        = 0,
	numeric timeout    = 0,
	string  sortOrder  = "",
	boolean ignoreCase = false,
	boolean asQuery    = false,
	boolean asStream   = getAsStream()
)
```

As you can see from the signature above, the first two arguments (`offset, max`) are used for pagination.  So you can easily paginate the result set.  The `timeout` argument can be used if the query is expected to be heavy duty, we want to make sure we timeout the execution (throws exception).  The `ignorecase` is only used for sorting orders.

Then we get to the last two arguments: `asQuery, asStream`.  By default the `list()` method will return an array of objects. However, if you want different results you can use this two modifiers to give you a ColdFusion query or a [Java stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html).

```javascript
// Listing
var results = c
     .like("name", "lui%")
     .list();

var c = newCriteria()
// only approved comments
.isTrue( "isApproved" )
// By Content?
.when( !isNull( arguments.contentID ) AND len( arguments.contentID ), function( c ){
	c.isEq( "relatedContent.contentID", contentID );
} )
// By Content Type Discriminator: class is a special hibernate deal
.when( !isNull( arguments.contentType ) AND len( arguments.contentType ), function( c ){
	c.createCriteria( "relatedContent" ).isEq( "class", contentType );
} )
// Site Filter
.when( len( arguments.siteID ), function( c ){
	c.joinTo( "relatedContent", "relatedContent" )
		.isEq( "relatedContent.site.siteID", siteID );
} );

// run criteria query and projections count
results.count    = c.count();
results.comments = c.list(
	offset   : arguments.offset,
	max      : arguments.max,
	sortOrder: "createdDate #arguments.sortOrder#",
	asQuery  : false
);
```

{% hint style="success" %}
**Tip:** You can call `count()` and `list()` on the same criteria, but due to the internal workings of Hibernate, you must call `count()` first, then `list()`.
{% endhint %}
