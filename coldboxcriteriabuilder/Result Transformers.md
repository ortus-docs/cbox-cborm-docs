Our criteria builder also supports the notion of projections (http://docs.jboss.org/hibernate/core/3.3/reference/en/html/querycriteria.html#querycriteria-projection). A projection is used to change the nature of the results, much how a result transformer does. However, there are several projection types you can use which are great for doing counts, distinct counts, max values, sums, averages and much more. This is great when you do paging as obviously you do not want to execute two queries: one for the pagination and another for the total reuslts count. Below are the available projections you can use:

| Transform | Description | Example |
| --- | --- | --- |
| avg | The name of the property to avg or a list or array of property names | *withProjections(avg="salary")* |
| count | The name of the property to count or a list or array of property names | *withProjections(count="comments")* |
| countDistinct | The name of the property to count distinct or a list or array of property names | *withProjections(countDistinct="email")* |
| distinct | The name of the property to do a distinct on, this can be a single property name a list or an array of property names | *withProjections(distinct="email")* |
| groupProperty | The name of the property to group by or a list or array of property names | *withProjections(groupproperty="lastName")* |
| max | The name of the property to max or a list or array of property names | *withProjections(max="lastLogin")* |
| min | The name of the property to min or a list or array of property names | *withProjections(min="cid")* |
| property | The name of the property to do a projected value on or a list or array of property names | *withProjections(property="firstname")* |
| sum | The name of the property to sum or a list or array of property names | *withProjections(sum="balance")* |
| rowCount | Do a row count on the criteria | *withProjections(rowcount=1)* |
| id | Return the projected identifier value | *withProjections(id=1)* |
| sqlProjection | Return projected value for sql fragment. Can accept a single config {sql,alias,property}, or an array of multiple configs. | *withProjections(sqlProjection={sql="SELECT count( * ) from blog where Year < 2006 and Author={alias}.Author", <br>alias="BlogPosts", <br>property="Author" })* |
| sqlGroupProjection | Return projected value for sql fragment with grouping. Can accept a single config( sql,alias,property,group}, or an array of multiple configs. | *withProjections(sqlGroupProjection={sql="SELECT count( * ) from blog where Year < 2006 and Author={alias}.Author", <br>alias="BlogPosts", <br>property="Author", <br>group="Author" })* |
| detachedSQLProjection | Creates a sqlProjection() based on Detached Criteria Builder | See [Detached Criteria Builder](https://github.com/ColdBox/cbox-cborm/wiki/ORM-Detached-Criteria-Builder) |

You will use them by passing them to the *withProjections()* method as arguments that match the projection type. The value of the argument is one, a list or an array of property names to run the projection on, with the exception of id and rowcount which take a boolean true. Also, you can pass in a string separated with a : to denote an alias on the property when doing the SQL. The alias can then be used with any restriction the criteria builder can use.

```javascript
Ex: avg="balance", avg="balance:myBalance", avg="balance, total", avg=["balance","total"]
```

> **INFO** If the :alias is not used, then the alias becomes the property name.

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