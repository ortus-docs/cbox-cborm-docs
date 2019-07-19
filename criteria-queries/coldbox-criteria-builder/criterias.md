# Criterias

To build our criteria queries we will mostly use the methods in the criteria object or go directly to the restrictions object for very explicit criterions as explained above. We will also go to the restrictions object when we do conjunctions and disjunctions, which are fancy words for AND's, OR's and NOT's. So to build criterias we will be calling these criterion methods and concatenate them in order to form a nice DSL language that describes what we will retrieve. Once we have added all the criteria then we can use several other concatenated methods to set executions options and then finally retrieve our results or do projections on our results.

So let's start with all the different supported criterion methods in the Criteria object, which are the most commonly used. If you need to use methods that are not in the Criteria object you will request them via the Restrictions object, which can proxy calls to the underlying Hibernate native Restrictions class \([http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html)\).

| Method | Description | Example |
| :--- | :--- | :--- |
| between\(property,minValue,maxValue\) | Where the property value is between two distinct values | _c.between\("age",10,30\);_ |
| conjunction\(required array restrictionValues\) | Group expressions together in a single conjunction \(A and B and C...\) and return the conjunction | _c.conjunction\( \[_  _c.restrictions.between\("balance",100,200\),_  _c.restrictions.lt\("salary",20000\) \] \);_ |
| disjunction\(required array restrictionValues\) | Group expressions together in a single disjunction \(A or B or C...\) | _c.disjunction\( \[_  _c.restrictions.between\("balance",100,200\),_  _c.restrictions.lt\("salary",20000\) \] \);_ |
| eqProperty\(property, otherProperty\) | Where one property must equal another | _c.eqProperty\("createDate","modifyDate"\);_ |
| eq\(property, value\) or isEq\(property,value\) | Where a property equals a particular value, you can also use eq\(\) | _c.eq\("age",30\);_ |
| gt\(property, value\) or isGT\(property, value\) | Where a property is greater than a particular value, you can also use gt\(\) | _c.gt\("publishedDate", now\(\) \);_ |
| gtProperty\(property,otherProperty\) | Where a one property must be greater than another | _c.gtProperty\("balance","overdraft"\);_ |
| ge\(property,value\) or isGE | Where a property is greater than or equal to a particular value, you can also use ge\(\) | _c.ge\("age",18\);_ |
| geProperty\(property, otherProperty\) | Where a one property must be greater than or equal to another | _c.geProperty\("balance","overdraft"\);_ |
| idEQ\(required any propertyValue\) | Where an objects id equals the specified value | _c.idEq\( 4 \);_ |
| ilike\(required string property, required string propertyValue\) | A case-insensitive 'like' expression | _c.ilike\("lastName", "maj%"\);_ |
| isIn\(required string property, required any propertyValue\) or in\(required string property, required any propertyValue\) | Where a property is contained within the specified list of values, the property value can be a collection \(struct\) or array or list, you can also use in\(\) | _c.isIn\( "id", \[1,2,3,4\] \);_ |
| isEmpty\(required string property\) | Where a collection property is empty | _c.isEmpty\("childPages"\);_ |
| isNotEmpty\(required string property\) | Where a collection property is not empty | _c.isNotEmpty\("childPages"\);_ |
| isFalse\(required string property\) | Where a collection property is false | _c.isFalse\("isPublished"\);_ |
| isNull\(required string property\) | Where a property is null | _c.isNull\("passwordProtection"\);_ |
| isNotNull\(required string property\) | Where a property is NOT null | _c.isNotNull\("publishedDate"\);_ |
| islt\(required string property, required any propertyValue\) or lt\(\) | Where a property is less than a particular value, you can also use lt\(\) | _c.isLT\("age", 40 \);_ |
| ltProperty\(required string property, required string otherProperty\) | Where a one property must be less than another | _c.ltProperty\("sum", "balance"\);_ |
| isle\(required string property, required any propertyValue\) or le\(\) | Where a property is less than or equal a particular value, you can also use le\(\) | _c.isLE\("age", 30\);_ |
| leProperty\(required string property, required string otherProperty\) | Where a one property must be less than or equal to another | _c.LeProperty\("balance","balance2"\);_ |
| like\(required string property, required string propertyValue\) | Equivalent to SQL like expression | _c.like\("content", "%search%"\);_ |
| ne\(required string property, required any propertyValue\) | Where a property does not equal a particular value | _c.ne\("isPublished", true\);_ |
| neProperty\(required string property, required any otherProperty\) | Where one property does not equal another | _c.neProperty\("password","passwordHash"\);_ |
| sizeEq\(required string property, required any propertyValue\) | Where a collection property's size equals a particular value | _c.sizeEq\("comments",30\);_ |
| sizeGT\(required string property, required any propertyValue\) | Where a collection property's size is greater than a particular value | _c.sizeGT\("children",5\);_ |
| sizeGE\(required string property, required any propertyValue\) | Where a collection property's size is greater than or equal to a particular value | _c.sizeGE\("children", 10\);_ |
| sizeLT\(required string property, required any propertyValue\) | Where a collection property's size is less than a particular value | _c.sizeLT\("childPages", 25 \);_ |
| sizeLE\(required string property, required any propertyValue\) | Where a collection property's size is less than or equal a particular value | _c.sizeLE\("childPages", 25 \);;_ |
| sizeNE\(required string property, required any propertyValue\) | Where a collection property's size is not equal to a particular value | _c.sizeNE\("childPages",0\);_ |
| sqlRestriction\(required string sql\) | Use arbitrary SQL to modify the resultset | _c.sqlRestriction\("char\_length\( lastName \) = 10"\);_ |
| and\(Criterion, Criterion, ...\) | Return the conjuction of N expressions as arguments | _c.and\( c.restrictions.eq\("name","luis"\), c.restrictions.gt\("age",30\) \);_ |
| or\(Criterion, Criterion, ….\) | Return the disjunction of N expressions as arguments | c.or\( c.restrictions.eq\("name","luis"\), c.restrictions.eq\("name", "joe"\) \) |
| not\(required any criterion\) or isNot\(\) | Return the negation of an expression | _c.isNot\( c.restrictions.eg\("age", 30\) \);_ |
| isTrue\(required string property\) | Returns if the property is true | _c.isTrue\("isPublished"\);_ |

> **Note** In some cases \(_isEq\(\)_, _isIn\(\)_, etc\), you may receive data type mismatch errors. These can be resolved by using [JavaCast](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7fbe.html) on your criteria value.

```javascript
c.isEq("userID", JavaCast( "int", 3 ));
```

You can also use the add\(\) method to add a manual restriction or array of restrictions to the criteria you are building.

```javascript
c.add( c.restrictions.eq("name","luis") )
```

But as you can see from the code, the facade methods are much nicer.

