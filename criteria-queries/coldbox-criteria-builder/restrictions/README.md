# Restrictions

The ColdBox restrictions class allows you to create criterions upon certain properties, associations and even SQL for your ORM entities. This is the meat and potatoes of criteria queries, where you build a set of criterion to match against or in SQL terms, your `WHERE` statements.

The ColdBox criteria class offers almost all of the criterion methods found in the native hibernate Restrictions class:

* [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html)
* [https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/criterion/Restrictions.html](https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/criterion/Restrictions.html)
* [https://docs.jboss.org/hibernate/orm/5.0/javadocs/org/hibernate/criterion/Restrictions.html](https://docs.jboss.org/hibernate/orm/5.0/javadocs/org/hibernate/criterion/Restrictions.html)

If there isn't one defined in the CFML equivalent then don't worry, just call it like is appears in the Javadocs and we will proxy the call to the native Hibernate class for you.

## Direct Reference

You can get a direct reference to the Restrictions class via the Base/Virtual ORM services \(`getRestrictions())`, or the Criteria object itself has a public property called `restrictions` which you can use rather easily. We prefer the latter approach. Now, please understand that the ColdBox Criteria Builder masks all this complexity and in very rare cases will you be going to our restrictions class directly. Most of the time you will just be happily concatenating methods on the Criteria Builder.

```javascript
// From base ORM service
var restrictions = service.getRestrictions()

// From Criteria Builder
newCriteria().restrictions
```

Ok, now that the formalities have been explained let's build some criterias.

## Building Restrictions

To build our criteria queries we will mostly use the methods in the criteria object or go directly to the restrictions object for very explicit criterions as explained above. We will also go to the restrictions object when we do conjunctions and disjunctions, which are fancy words for AND's, OR's and NOT's. So to build criterias we will be calling these criterion methods and concatenate them in order to form a nice DSL language that describes what we will retrieve. Once we have added all the criteria then we can use several other concatenated methods to set executions options and then finally retrieve our results or do projections on our results.

| Method | Description | Example |
| :--- | :--- | :--- |
| `between(property,minValue,maxValue)` | Where the property value is between two distinct values | _c.between\("age",10,30\);_ |
| `conjunction(required array restrictionValues)` | Group expressions together in a single conjunction \(A and B and C...\) and return the conjunction | _c.conjunction\( \[_  _c.restrictions.between\("balance",100,200\),_  _c.restrictions.lt\("salary",20000\) \] \);_ |
| `disjunction(required array restrictionValues)` | Group expressions together in a single disjunction \(A or B or C...\) | _c.disjunction\( \[_  _c.restrictions.between\("balance",100,200\),_  _c.restrictions.lt\("salary",20000\) \] \);_ |
| `eqProperty(property, otherProperty)` | Where one property must equal another | _c.eqProperty\("createDate","modifyDate"\);_ |
| `eq(property, value) or isEq(property,value)` | Where a property equals a particular value, you can also use eq\(\) | _c.eq\("age",30\);_ |
| `gt(property, value) or isGT(property, value)` | Where a property is greater than a particular value, you can also use gt\(\) | _c.gt\("publishedDate", now\(\) \);_ |
| `gtProperty(property,otherProperty)` | Where a one property must be greater than another | _c.gtProperty\("balance","overdraft"\);_ |
| `ge(property,value) or isGE` | Where a property is greater than or equal to a particular value, you can also use ge\(\) | _c.ge\("age",18\);_ |
| `geProperty(property, otherProperty)` | Where a one property must be greater than or equal to another | _c.geProperty\("balance","overdraft"\);_ |
| `idEQ(required any propertyValue)` | Where an objects id equals the specified value | _c.idEq\( 4 \);_ |
| `ilike(required string property, required string propertyValue)` | A case-insensitive 'like' expression | _c.ilike\("lastName", "maj%"\);_ |
| `isIn(required string property, required any propertyValue) or in(required string property, required any propertyValue)` | Where a property is contained within the specified list of values, the property value can be a collection \(struct\) or array or list, you can also use in\(\) | _c.isIn\( "id", \[1,2,3,4\] \);_ |
| `isEmpty(required string property)` | Where a collection property is empty | _c.isEmpty\("childPages"\);_ |
| `isNotEmpty(required string property)` | Where a collection property is not empty | _c.isNotEmpty\("childPages"\);_ |
| `isFalse(required string property)` | Where a collection property is false | _c.isFalse\("isPublished"\);_ |
| `isNull(required string property)` | Where a property is null | _c.isNull\("passwordProtection"\);_ |
| `isNotNull(required string property)` | Where a property is NOT null | _c.isNotNull\("publishedDate"\);_ |
| `islt(required string property, required any propertyValue) or lt()` | Where a property is less than a particular value, you can also use lt\(\) | _c.isLT\("age", 40 \);_ |
| `ltProperty(required string property, required string otherProperty)` | Where a one property must be less than another | _c.ltProperty\("sum", "balance"\);_ |
| `isle(required string property, required any propertyValue) or le()` | Where a property is less than or equal a particular value, you can also use le\(\) | _c.isLE\("age", 30\);_ |
| `leProperty(required string property, required string otherProperty)` | Where a one property must be less than or equal to another | _c.LeProperty\("balance","balance2"\);_ |
| `like(required string property, required string propertyValue)` | Equivalent to SQL like expression | _c.like\("content", "%search%"\);_ |
| `ne(required string property, required any propertyValue)` | Where a property does not equal a particular value | _c.ne\("isPublished", true\);_ |
| `neProperty(required string property, required any otherProperty)` | Where one property does not equal another | _c.neProperty\("password","passwordHash"\);_ |
| `sizeEq(required string property, required any propertyValue)` | Where a collection property's size equals a particular value | _c.sizeEq\("comments",30\);_ |
| `sizeGT(required string property, required any propertyValue)` | Where a collection property's size is greater than a particular value | _c.sizeGT\("children",5\);_ |
| `sizeGE(required string property, required any propertyValue)` | Where a collection property's size is greater than or equal to a particular value | _c.sizeGE\("children", 10\);_ |
| `sizeLT(required string property, required any propertyValue)` | Where a collection property's size is less than a particular value | _c.sizeLT\("childPages", 25 \);_ |
| `sizeLE(required string property, required any propertyValue)` | Where a collection property's size is less than or equal a particular value | _c.sizeLE\("childPages", 25 \);;_ |
| `sizeNE(required string property, required any propertyValue)` | Where a collection property's size is not equal to a particular value | _c.sizeNE\("childPages",0\);_ |
| `sql(required sql, params)` | Use arbitrary SQL to modify the resultset | _c.sql\("char\_length\( lastName \) = 10"\);_ |
| `and(Criterion, Criterion, ...)` | Return the conjuction of N expressions as arguments | _c.and\( c.restrictions.eq\("name","luis"\), c.restrictions.gt\("age",30\) \);_ |
| `or(Criterion, Criterion, ….)` | Return the disjunction of N expressions as arguments | c.or\( c.restrictions.eq\("name","luis"\), c.restrictions.eq\("name", "joe"\) \) |
| `not(required any criterion) or isNot()` | Return the negation of an expression | _c.isNot\( c.restrictions.eg\("age", 30\) \);_ |
| `isTrue(required string property)` | Returns if the property is true | _c.isTrue\("isPublished"\);_ |

{% hint style="info" %}
In some cases \(`isEq(), isIn(),` etc\), you may receive data type mismatch errors. These can be resolved by using JavaCast on your criteria value or use our auto caster methods: `idCast(), autoCast()`
{% endhint %}

```javascript
c.isEq("userID", idCast( 3 ) );
```

You can also use the `add()` method to add a manual restriction or array of restrictions to the criteria you are building.

```javascript
c.add( c.restrictions.eq("name","luis") )
```

But as you can see from the code, the facade methods are much nicer.

## Dynamic Negation

Every restriction method you see above or in the docs can also be negated very easily by just prefixing the method with a `not` .

```javascript
c
    .notEq("userID", idCast( 3 ) )
    .notBetween()
    .notIsTrue()
    .notIsFalse();
```

## Fluent If Statements - `when()`

There comes times where you need some if statements in order to add criterias based on incoming input.  That's ok, but we can do better by adding a `when( test, target )` function that will evaluate the `test` argument or expression.  If it evaluates to true then the target closure is called for you with the criteria object so you can do your criterias:

```javascript
newCriteria()
	.when( isBoolean( arguments.isPublished ), function( c ){
		// Published process
		c
			.isEq( "isPublished", isPublished )
			.when( isPublished, function( c ){
				c.isLt( "publishedDate", now() )
				.$or( 
					c.restrictions.isNull( "expireDate" ), 
					c.restrictions.isGT( "expireDate", now() ) 
				)
				.isEq( "passwordProtection","" );
			})
		}
	} )
  .when( !isNull( arguments.showInSearch ), function( c ){
  		c.isEq( "showInSearch", showInSearch );
   } )
   .when( arguments.isActive, function( c ){
   	 	c.isTrue( "isActive" );
   })
  .list()
```



