#Criterias
All of the same criterias defined in Criteria Builder can be utilized in a Detached Criteria Builder as well. Easy, huh?

Here they go again...

To build our criteria queries we will mostly use the methods in the criteria object or go directly to the restrictions object for very explicit criterions as explained above. We will also go to the restrictions object when we do conjunctions and disjunctions, which are fancy words for AND's, OR's and NOT's. So to build criterias we will be calling these criterion methods and concatenate them in order to form a nice DSL language that describes what we will retrieve. Once we have added all the criteria then we can use several other concatenated methods to set executions options and then finally retrieve our results or do projections on our results. 

So let's start with all the different supported criterion methods in the Criteria object, which are the most commonly used. If you need to use methods that are not in the Criteria object you will request them via the Restrictions object, which can proxy calls to the underlying Hibernate native Restrictions class (http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html).

| Method | Description | Example |
| --- | --- | --- |
| between(property,minValue,maxValue) | Where the property value is between two distinct values | *c.between("age",10,30);* |
| conjunction(required array restrictionValues) | Group expressions together in a single conjunction (A and B and C...) and return the conjunction | *c.conjunction( [ <br>c.restrictions.between("balance",100,200), <br>c.restrictions.lt("salary",20000) ] );* |
| disjunction(required array restrictionValues) | Group expressions together in a single disjunction (A or B or C...) | *c.disjunction( [ <br>c.restrictions.between("balance",100,200), <br>c.restrictions.lt("salary",20000) ] );* |
| eqProperty(property, otherProperty) | Where one property must equal another | *c.eqProperty("createDate","modifyDate");* |
| eq(property, value) or isEq(property,value) | Where a property equals a particular value, you can also use eq() | *c.eq("age",30);* |
| gt(property, value) or isGT(property, value) | Where a property is greater than a particular value, you can also use gt() | *c.gt("publishedDate", now() );* |
| gtProperty(property,otherProperty) | Where a one property must be greater than another | *c.gtProperty("balance","overdraft");* |
| ge(property,value) or isGE | Where a property is greater than or equal to a particular value, you can also use ge() | *c.ge("age",18);* |
| geProperty(property, otherProperty) | Where a one property must be greater than or equal to another | *c.geProperty("balance","overdraft");* |
| idEQ(required any propertyValue) | Where an objects id equals the specified value | *c.idEq( 4 );* |
| ilike(required string property, required string propertyValue) | A case-insensitive 'like' expression | *c.ilike("lastName", "maj%");* |
| isIn(required string property, required any propertyValue) or in(required string property, required any propertyValue) | Where a property is contained within the specified list of values, the property value can be a collection (struct) or array or list, you can also use in() | *c.isIn( "id", [1,2,3,4] );* |
| isEmpty(required string property) | Where a collection property is empty | *c.isEmpty("childPages");* |
| isNotEmpty(required string property) | Where a collection property is not empty | *c.isNotEmpty("childPages");* |
| isFalse(required string property) | Where a collection property is false | *c.isFalse("isPublished");* |
| isNull(required string property) | Where a property is null | *c.isNull("passwordProtection");* |
| isNotNull(required string property) | Where a property is NOT null | *c.isNotNull("publishedDate");* |
| islt(required string property, required any propertyValue) or lt() | Where a property is less than a particular value, you can also use lt() | *c.isLT("age", 40 );* |
| ltProperty(required string property, required string otherProperty) | Where a one property must be less than another | *c.ltProperty("sum", "balance");* |
| isle(required string property, required any propertyValue) or le() | Where a property is less than or equal a particular value, you can also use le() | *c.isLE("age", 30);* |
| leProperty(required string property, required string otherProperty) | Where a one property must be less than or equal to another | *c.LeProperty("balance","balance2");* |
| like(required string property, required string propertyValue) | Equivalent to SQL like expression | *c.like("content", "%search%");* |
| ne(required string property, required any propertyValue) | Where a property does not equal a particular value | *c.ne("isPublished", true);* |
| neProperty(required string property, required any otherProperty) | Where one property does not equal another | *c.neProperty("password","passwordHash");* |
| sizeEq(required string property, required any propertyValue) | Where a collection property's size equals a particular value | *c.sizeEq("comments",30);* |
| sizeGT(required string property, required any propertyValue) | Where a collection property's size is greater than a particular value | *c.sizeGT("children",5);* |
| sizeGE(required string property, required any propertyValue) | Where a collection property's size is greater than or equal to a particular value | *c.sizeGE("children", 10);* |
| sizeLT(required string property, required any propertyValue) | Where a collection property's size is less than a particular value | *c.sizeLT("childPages", 25 );* |
| sizeLE(required string property, required any propertyValue) | Where a collection property's size is less than or equal a particular value | *c.sizeLE("childPages", 25 );;* |
| sizeNE(required string property, required any propertyValue) | Where a collection property's size is not equal to a particular value | *c.sizeNE("childPages",0);* |
| sqlRestriction(required string sql) | Use arbitrary SQL to modify the resultset | *c.sqlRestriction("char_length( lastName ) = 10");* |
| and(Criterion, Criterion, ...) | Return the conjuction of N expressions as arguments | *c.and( c.restrictions.eq("name","luis"), c.restrictions.gt("age",30) );* |
| or(Criterion, Criterion, ….) | Return the disjunction of N expressions as arguments | c.or( c.restrictions.eq("name","luis"), c.restrictions.eq("name", "joe") ) |
| not(required any criterion) or isNot() | Return the negation of an expression | *c.isNot( c.restrictions.eg("age", 30) );* |
| isTrue(required string property) | Returns if the property is true | *c.isTrue("isPublished");* |

> **Note** In some cases (*isEq()*, *isIn()*, etc), you may receive data type mismatch errors. These can be resolved by using [JavaCast](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7fbe.html) on your criteria value. 

```javascript
c.isEq("userID", JavaCast( "int", 3 ));
```

You can also use the add() method to add a manual restriction or array of restrictions to the criteria you are building.

```javascript
c.add( c.restrictions.eq("name","luis") )
```

But as you can see from the code, the facade methods are much nicer.