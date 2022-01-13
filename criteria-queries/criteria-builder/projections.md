# Projections & Aggregates

Hibernate also supports the ability to work with sql projections and sql aggregates.  Instead of treating the results as an array of objects or a stream of objects you can return the result as a row with columns of data.

```javascript
var statusReport = c
    .withProjections( count: "isActive:authors", groupProperty: "isActive" )
    .asStruct()
    .list();
```

This is great for API driven applications as you DON"T have to retrieve the entire object graphs, you can decide which columns to bring back and return an array of structs with lightening speed by just using the `asStruct()` modifier.

There are several projection types you can use which are great for doing counts, distinct counts, max values, sums, averages and much more.&#x20;

## withProjections()

The method in the criteria builder that will allow you to add projections is called `withProjections()`.  You will then use it's arguments to tell Hibernate what projection or aggregates to compile into the query.  Here is the method signature:

```java
/**
 * Setup projections for this criteria query, you can pass one or as many projection arguments as you like.
 * The majority of the arguments take in the property name to do the projection on, which will also use that as the alias for the column
 * or you can pass an alias after the property name separated by a : Ex: projections(avg="balance:avgBalance")
 * The alias on the projected value can be referred to in restrictions or orderings.
 * Please also note that the resulting array locations are done in alphabetical order of the arguments.
 *
 * @avg The name of the property to avg or a list or array of property names
 * @count The name of the property to count or a list or array of property names
 * @countDistinct The name of the property to count distinct or a list or array of property names
 * @distinct The name of the property to do a distinct on, this can be a single property name a list or an array of property names
 * @groupProperty The name of the property to group by or a list or array of property names
 * @id The projected identifier value
 * @max The name of the property to max or a list or array of property names
 * @min The name of the property to min or a list or array of property names
 * @property The name of the property to do a projected value on or a list or array of property names
 * @rowCount Do a row count on the criteria
 * @sum The name of the property to sum or a list or array of property names
 * @sqlProjection Do a projection based on arbitrary SQL string
 * @sqlGroupProjection Do a projection based on arbitrary SQL string, with grouping
 * @detachedSQLProjection Do a projection based on a DetachedCriteria builder config
 */
any function withProjections(
	string avg,
	string count,
	string countDistinct,
	any distinct,
	string groupProperty,
	boolean id,
	string max,
	string min,
	string property,
	boolean rowCount,
	string sum,
	any sqlProjection,
	any sqlGroupProjection,
	any detachedSQLProjection
){
```

### Arguments

Below are the available projections you can use from this method

| Transform               | Description                                                                                                                                    | Example                                                                                                                                                                                                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `avg`                   | The name of the property to avg or a list or array of property names                                                                           | _`withProjections(avg="salary")`_                                                                                                                                                                                                                                            |
| `count`                 | The name of the property to count or a list or array of property names                                                                         | _`withProjections(count="comments")`_                                                                                                                                                                                                                                        |
| `countDistinct`         | The name of the property to count distinct or a list or array of property names                                                                | _`withProjections(countDistinct="email")`_                                                                                                                                                                                                                                   |
| `distinct`              | The name of the property to do a distinct on, this can be a single property name a list or an array of property names                          | _`withProjections(distinct="email")`_                                                                                                                                                                                                                                        |
| `groupProperty`         | The name of the property to group by or a list or array of property names                                                                      | _`withProjections(groupproperty="lastName")`_                                                                                                                                                                                                                                |
| `id`                    | Return the projected identifier value                                                                                                          | _`withProjections(id=true)`_                                                                                                                                                                                                                                                 |
| `max`                   | The name of the property to max or a list or array of property names                                                                           | _`withProjections(max="lastLogin")`_                                                                                                                                                                                                                                         |
| `min`                   | The name of the property to min or a list or array of property names                                                                           | _`withProjections(min="cid")`_                                                                                                                                                                                                                                               |
| `property`              | The name of the property to do a projected value on or a list or array of property names                                                       | _`withProjections(property="firstname")`_                                                                                                                                                                                                                                    |
| `rowCount`              | Do a row count on the criteria                                                                                                                 | _`withProjections(rowcount=true)`_                                                                                                                                                                                                                                           |
| `sum`                   | The name of the property to sum or a list or array of property names                                                                           | _`withProjections(sum="balance")`_                                                                                                                                                                                                                                           |
| `sqlProjection`         | Return projected value for sql fragment. Can accept a single config {sql,alias,property}, or an array of multiple configs.                     | <p><em><code>withProjections(sqlProjection={sql="SELECT count(</code></em><code>  ) from blog where Year &#x3C; 2006 and Author={alias}.Author",</code> <br><code>alias="BlogPosts",</code> <br><code>property="Author" })*</code></p>                                       |
| `sqlGroupProjection`    | Return projected value for sql fragment with grouping. Can accept a single config( sql,alias,property,group}, or an array of multiple configs. | <p><em><code>withProjections(sqlGroupProjection={sql="SELECT count(</code></em><code>  ) from blog where Year &#x3C; 2006 and Author={alias}.Author",</code> <br><code>alias="BlogPosts",</code> <br><code>property="Author",</code> <br><code>group="Author" })*</code></p> |
| `detachedSQLProjection` | Creates a sqlProjection() based on Detached Criteria Builder                                                                                   | See [Detached Criteria Builder](https://github.com/ColdBox/cbox-cborm/wiki/ORM-Detached-Criteria-Builder)                                                                                                                                                                    |

The value of the arguments is one, a list or an array of property names to run the projection on, with the exception of `id` and `rowcount` which take a `boolean` true.&#x20;

### Aliases

Also, you can pass in a string separated with a **:** to denote an alias on the property when doing the SQL. The alias can then be used with any restriction the criteria builder can use.

```javascript
Ex: 
avg="balance",
avg="balance:myBalance", // balance as myBalance
avg="balance, total", 
avg=["balance","total"]
```

{% hint style="info" %}
If the `:alias` is not used, then the alias becomes the property name.
{% endhint %}

### Examples

```javascript
// Using native approach for one projection only
var results = c.like("firstName","Lui%")
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     .setProjection( c.projections.rowCount() )
     .get();

// Using the withProjections() method, which enables you to do more than 1 projection
var results = c.like("firstName","Lui%")
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     .withProjections(rowCount=1)
     .get();

var results = c.like("firstName","Lui%")
     .and( 
          c.restrictions.between( "balance", 200, 5000),
          c.restrictions.eq("department", "development")
     )
     .withProjections(avg="balance,total",max="total:maxTotal")
     .gt("maxTotal",500)
     .list();
     
var results = c
     .withProjections( property="id,name,username" )
     .isTrue( "isActive" )
     .asStruct()
     .list();
```

## Projections

Here is a detail overview of each projection type.

### avg

The name of the property to average or a list or array of property names to average on

```javascript
// projection
withProjections( avg = "salary" )
// Produces
select avg( salary ) from User

// projection with alias
withProjections( avg = "salary:totalSalary" )
// Produces
select avg( salary ) as totalSalary from User

// Multiple projections
withProjections( avg = "salary,balance" )
// Produces
select avg( salary ), avg( balance ) from User
```

### count

The name of the property to count on or a list or array of property names to count on

```javascript
// projection
withProjections( count = "id" )
// Produces
select count( id ) from User

// projection with alias
isTrue( "isActive" )
    .withProjections( count = "id:TotalUsers" )
// Produces
select count( id ) as totalUsers from User
where isActive = true
```

### countDistinct

Get a distinct count on a property or list of properties

```javascript
// projection
withProjections( distinctCount = "city:cities" )
// Produces
SELECT COUNT( DISTINCT city ) as cities
FROM User;
```

### distinct

Distinct is used to return only distinct (different) values from a property or list of properties

```javascript
// projection
withProjections( distinct = "city" )
// Produces
SELECT DISTINCT city
FROM User;
```

### groupProperty

Which properties to group in the SQL statement.

```javascript
// project users by country
withProjections( property : "country", count : "id" , groupProperty : "country" )

// Produces
SELECT COUNT(id), Country
FROM User
GROUP BY Country;
```

### id

Return an array of IDs of the current projected entity. Let's say we want all the user id's that have never logged in to the system. So we can use that to send them mails.

```javascript
isNull( "lastLogin" )
    .withProjections( id : true )
    .list();

// Produces
SELECT id
FROM User
WHERE lastLogin is null
```

### sqlProjection/sqlGroupProjection

Do a projection based on arbitrary SQL and SQL grouping strings.  The value can be a single struct or an array of structs with the following pattern:

* `sql` - The raw sql to execute
* `alias` - The aliases to apply
* `property` - The property projected on
* `group` - What to group on if needed

```javascript
.withProjections(
	groupProperty = "catid",
	sqlProjection = [
		{
			sql      : "count( category_id )",
			alias    : "count",
			property : "catid"
		}
	],
	sqlGroupProjection = [
		{
			sql      : "year( modifydate )",
			group    : "year( modifydate )",
			alias    : "modifiedDate",
			property : "id"
		},
		{
			sql      : "dateDiff('2021-12-31 23:59:59','2021-12-30')",
			group    : "dateDiff('2021-12-31 23:59:59','2021-12-30')",
			alias    : "someDateDiff",
			property : "id"
		}
	]
)
.asStruct()
.peek( function( c ){
	debug( c.getSql( true, true ) );
} )
.list();
```

This will produce the following SQL

```sql
select
    this.category_id as y0_,
    count(category_id) as count,
    year(modifydate) as modifiedDate,
    dateDiff('2021-12-31 23:59:59', '2021-12-30') as someDateDiff
from
    categories this_
group by
    this.category_id,
    year(modifydate),
    dateDiff('2021-12-31 23:59:59', '2021-12-30')
```
