#Getting Started
A criteria builder can be requested from our Base ORM services or a virtual service, which will bind itself automatically to the binded entity, by calling on their newCriteria() method. The corresponding class is: cborm.models.CriteriaBuilder

The arguments for the newCriteria() method are:

| Argument | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | true | --- | The name of the entity to bind this criteria builder with, the initial pivot. |
| useQueryCaching | useQueryCaching | false | false | To allow for query caching of *list()* operations |
| queryCacheRegion | string | false | criteria.{entityName} | The name of the cache region to use |

If you call newCriteria() from a virtual service layer, then you don't pass the entityName argument as it roots itself automatically.

Examples

```javascript
// Base ORM Service
c = newCriteria( 'entityName' );
// Virtual
c = newCriteria();

// Examples
var results = c.like("firstName","Lui%")
     .maxResults( 50 )
     .order("balance","desc")
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     .list();

// with pagination
var results = c.like("firstName","Lui%")
     .order("balance","desc")
     .and( 
          c.restrictions.between( "balance", 200, 300),
          c.restrictions.eq("department", "development")
     )
     .list(max=50,offset=20);

// more complex
var results = c.in("name","luis,fred,joe")
     .OR( c.restrictions.isNull("age"), c.restrictions.eq("age",20) )
     .list();
```

Once you have an instance of the Criteria Builder class you can start adding restrictions, projections and configuration data for your query. All by concatenating methods in a nice programmatic DSL. Once all the restrictions, projections and/or configuration data are in place, you will execute the query/projections using our result methods. Please note that you can request as many new criteria builders as you like and each of them will execute different queries. So let's start with the restrictions.