# Detached Criteria Builder

Hibernate also supports the concept of [detached criterias](https://docs.jboss.org/hibernate/orm/5.4/userguide/html\_single/Hibernate\_User\_Guide.html#pc-detach).  They are most useful for join conditions, subselects, subqueries and to query outside the current session.  As usual Hibernate has funny names for features that SQL offers without all these mixup of words.  Plain and simple detached criterias are used for doing complex queries where you need to do groupings, sub selects, sub queries and more.

{% embed url="https://docs.jboss.org/hibernate/stable/orm/javadocs/org/hibernate/criterion/DetachedCriteria.html" %}

> Some applications need to create criteria queries in "detached mode", where the Hibernate session is not available. Then you can execute the search in an arbitrary session.

To create an instance of Detached Criteria Builder, simply call the _`createSubcriteria()`_ method on your existing criteria object and incorporate it via the `add()` method.

```javascript
var c = newCriteria();

c.add(
    c.createSubCriteria()
);
```

Once you have an instance of the sub criteria, then just add in your restrictions:

```javascript
prc.pageTitle = "Criteria Builder - Subquery";
var c = carService.newCriteria();

// add subquery
prc.results = c.add(
		c.createSubcriteria( "Car", "carstaff" )
			// the property in the subquery to use or retrieve
			.withProjections( property : "CarID" )
			.joinTo( "carstaff.SalesPeople", "staff" )
				.joinTo( "staff.Position", "position" )
					.isEq( "position.LongName", "Finance Officer" )
		// then we use propertyIn() to get all the Cars from the sub query
		.propertyIn( "CarID" )
)
.list()
// Map it to the memento, so we can see it nicely.
.map( function( item ){
	return item.getMemento();
} );
```

{% hint style="success" %}
**Hint**: Remember you can use the `getSQL()` method to get the actual SQL that will be produced.  You can even add the argument: `returnExecutableSql` and get the actual executable SQL.
{% endhint %}

With the  Detached Criteria Builder, you can expand the power and flexibility of your criteria queries with support for criteria and projection subqueries, all while using the same intuitive patterns of Criteria Builder. No fuss, just more flexibility and control for your criteria queries!

For more information about Detached Criteria and Subqueries, check out the following Hibernate documentation resources:

1. Hibernate Docs: [http://docs.jboss.org/hibernate/orm/3.5/reference/en/html/querycriteria.html#querycriteria-detachedqueries](http://docs.jboss.org/hibernate/orm/3.5/reference/en/html/querycriteria.html#querycriteria-detachedqueries)
2. Hibernate DetachedCriteria: [http://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/criterion/DetachedCriteria.html](http://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/criterion/DetachedCriteria.html)
3. Hibernate Subqueries: [http://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/criterion/Subqueries.html](http://docs.jboss.org/hibernate/orm/3.5/api/org/hibernate/criterion/Subqueries.html)

{% hint style="info" %}
The best place to see all of the functionality of the cborm Detached Criteria Builder is to check out the latest API Docs: [https://apidocs.ortussolutions.com/#/coldbox-modules/cborm/](https://apidocs.ortussolutions.com/#/coldbox-modules/cborm/)
{% endhint %}
