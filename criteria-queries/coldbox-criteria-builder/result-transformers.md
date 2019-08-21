# Projections

Our criteria builder also supports the notion of projections \([http://docs.jboss.org/hibernate/core/3.3/reference/en/html/querycriteria.html\#querycriteria-projection](http://docs.jboss.org/hibernate/core/3.3/reference/en/html/querycriteria.html#querycriteria-projection)\). A projection is used to change the nature of the results, much how a result transformer does. However, there are several projection types you can use which are great for doing counts, distinct counts, max values, sums, averages and much more. This is great when you do paging as obviously you do not want to execute two queries: one for the pagination and another for the total results count. However, it can be used to retrieve HQL maps of properties, do aggregate functions and much more.

Below are the available projections you can use:

| Transform | Description | Example |
| :--- | :--- | :--- |
| avg | The name of the property to avg or a list or array of property names | _`withProjections(avg="salary")`_ |
| count | The name of the property to count or a list or array of property names | _`withProjections(count="comments")`_ |
| countDistinct | The name of the property to count distinct or a list or array of property names | _`withProjections(countDistinct="email")`_ |
| distinct | The name of the property to do a distinct on, this can be a single property name a list or an array of property names | _`withProjections(distinct="email")`_ |
| groupProperty | The name of the property to group by or a list or array of property names | _`withProjections(groupproperty="lastName")`_ |
| max | The name of the property to max or a list or array of property names | _`withProjections(max="lastLogin")`_ |
| min | The name of the property to min or a list or array of property names | _`withProjections(min="cid")`_ |
| property | The name of the property to do a projected value on or a list or array of property names | _`withProjections(property="firstname")`_ |
| sum | The name of the property to sum or a list or array of property names | _`withProjections(sum="balance")`_ |
| rowCount | Do a row count on the criteria | _`withProjections(rowcount=1)`_ |
| id | Return the projected identifier value | _`withProjections(id=1)`_ |
| sqlProjection | Return projected value for sql fragment. Can accept a single config {sql,alias,property}, or an array of multiple configs. | _`withProjections(sqlProjection={sql="SELECT count(`_  `) from blog where Year < 2006 and Author={alias}.Author",  alias="BlogPosts",  property="Author" })*` |
| sqlGroupProjection | Return projected value for sql fragment with grouping. Can accept a single config\( sql,alias,property,group}, or an array of multiple configs. | _`withProjections(sqlGroupProjection={sql="SELECT count(`_  `) from blog where Year < 2006 and Author={alias}.Author",  alias="BlogPosts",  property="Author",  group="Author" })*` |
| detachedSQLProjection | Creates a sqlProjection\(\) based on Detached Criteria Builder | See [Detached Criteria Builder](https://github.com/ColdBox/cbox-cborm/wiki/ORM-Detached-Criteria-Builder) |

You will use them by passing them to the `withProjections`_`()`_ method as arguments that match the projection type. The value of the argument is one, a list or an array of property names to run the projection on, with the exception of `id` and `rowcount` which take a `boolean` true. 

### Aliases

Also, you can pass in a string separated with a : to denote an alias on the property when doing the SQL. The alias can then be used with any restriction the criteria builder can use.

```javascript
Ex: avg="balance", avg="balance:myBalance", avg="balance, total", avg=["balance","total"]
```

{% hint style="info" %}
If the `:alias` is not used, then the alias becomes the property name.
{% endhint %}

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
```

