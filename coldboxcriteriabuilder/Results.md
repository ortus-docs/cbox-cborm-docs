Once you have concatenated criterias together, you can execute the query via the execution methods. Please remember that these methods return the results, so they must be executed last.

| Method | Description | Example |
| --- | --- | --- |
| list(max, offset, timeout, sortOrder, ignoreCase, asQuery=false) | Execute the criterias and give you the results. | *list(), list(max=50,offset=51,timeout=3000,ignoreCase=true)* |
| get() | Retrieve one result only. | *get()* |
| count() | Does a projection on the given criteria query and gives you the row count only, great for pagination totals or running counts. Note, count() can't be called on a criteria after list() has been executed. | *count()* |

```javascript
// Get
var results = c.idEq( 4 ).get();

// Listing
var results = c.like("name", "lui%")
     .list();

// Count via projections of all users that start with the letter 'L'
var count = c.ilike("name","L%").count();
```

> **Note** You can call *count()* and *list()* on the same criteria, but due to the internal workings of Hibernate, you must call *count()* first, then *list()*.
