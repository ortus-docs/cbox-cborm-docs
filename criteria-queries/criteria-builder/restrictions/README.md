# Restrictions

The ColdBox restrictions class allows you to create criterions upon certain properties, associations and even SQL for your ORM entities. This is the meat and potatoes of criteria queries, where you build a set of criteria to match against or in SQL terms, your `WHERE` statements.

The ColdBox criteria class offers most of the criterion methods found in the native hibernate Restrictions class:

* [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html)
* [https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/criterion/Restrictions.html](https://docs.jboss.org/hibernate/core/4.3/javadocs/org/hibernate/criterion/Restrictions.html)
* [https://docs.jboss.org/hibernate/orm/5.0/javadocs/org/hibernate/criterion/Restrictions.html](https://docs.jboss.org/hibernate/orm/5.0/javadocs/org/hibernate/criterion/Restrictions.html)

If one isn't defined in the CFML equivalent, just call it as it appears in the Javadocs and we will proxy the call to the native Hibernate class for you.

## Direct Reference

You can get a direct reference to the Restrictions class via the Base/Virtual ORM services (`getRestrictions())`, or the Criteria object itself has a public property called `restrictions` which you can use rather easily. We prefer the latter approach. Now, please understand that the ColdBox Criteria Builder masks all this complexity and in very rare cases will you be going to our restrictions class directly. Most of the time you will just be happily concatenating methods on the Criteria Builder.

```javascript
// From base ORM service
var restrictions = service.getRestrictions()

// From Criteria Builder
newCriteria().restrictions
```

Ok, now that the formalities have been explained let's build some criterias.

## Building Restrictions

To build our criteria queries we can use the methods in the criteria object or go directly to the restrictions object for very explicit criterions as explained above. We will also go to the restrictions object when we do conjunctions and disjunctions, which are fancy words for AND's, OR's and NOT's. To build criterias we will be calling these criterion methods and concatenate them in order to form a nice DSL language that describes what we will retrieve. Once we have added all the criteria then we can use several other concatenated methods to set executions options and then finally retrieve our results or do projections on our results.

### between( property, minValue, maxValue )

Where the property value is between two distinct values

```javascript
c.between("age",10,30);
```

### conjunction( required array restrictionValues )

Group expressions together in a single conjunction (A and B and C...) and return the conjunction

```javascript
c.conjunction( [  
    c.restrictions.between("balance",100,200),
    c.restrictions.lt("salary",20000) 
] );
```

### disjunction( required array restrictionValues )

Group expressions together in a single disjunction (A or B or C...)

```javascript
c.disjunction( [  
    c.restrictions.between("balance",100,200),
    c.restrictions.lt("salary",20000) 
] );
```

### isEQ( property, value )

Where a property equals a particular value, you can also use eq()

```javascript
c.eq("age",30);
```

### isGT( property, value )

Where a property is greater than a particular value, you can also use gt()

```javascript
c.gt("publishedDate", now() );
```

### gtProperty( property,otherProperty )

Where a one property must be greater than another

```javascript
c.gtProperty("balance","overdraft");
```

### isGE( property,value )

Where a property is greater than or equal to a particular value, you can also use ge()

```javascript
c.ge("age",18);
```

### geProperty( property, otherProperty )

Where one property must be greater than or equal to another

```javascript
c.geProperty("balance","overdraft");
```

### idEQ( required any propertyValue )

Where an object's id equals the specified value

```javascript
c.idEq( 4 );
```

### iLike( required string property, required string propertyValue )

A case-insensitive 'like' expression

```javascript
c.iLike("lastName", "maj%");
```

### isIn( required string property, required any propertyValue )

Where a property is contained within the specified list of values, the property value can be a collection (struct) or array or list, you can also use in()

```javascript
c.isIn( "id", [1,2,3,4] );
```

### isEmpty( required string property )

Where a collection property is empty

```javascript
c.isEmpty("childPages");
```

### isNotEmpty( required string property )

Where a collection property is not empty

```javascript
c.isNotEmpty("childPages");
```

### isFalse( required string property )

Where a collection property is false

```javascript
c.isFalse("isPublished");
```

### isNull( required string property )

Where a property is null

```javascript
c.isNull("passwordProtection");
```

### isNotNull( required string property )

Where a property is NOT null

```javascript
c.isNotNull("publishedDate");
```

### isLT( required string property, required any propertyValue )

Where a property is less than a particular value, you can also use lt()

```javascript
c.isLT("age", 40 );
```

### ltProperty( required string property, required string otherProperty )

Where a one property must be less than another

```javascript
c.ltProperty("sum", "balance");
```

### isLE( required string property, required any propertyValue )

Where a property is less than or equal a particular value, you can also use le()

```javascript
c.isLE("age", 30);
```

### leProperty( required string property, required string otherProperty )

Where a one property must be less than or equal to another

```javascript
c.LeProperty("balance","balance2");
```

### like( required string property, required string propertyValue )

Equivalent to SQL like expression

```javascript
c.like("content", "%search%");
```

### ne( required string property, required any propertyValue )

Where a property does not equal a particular value

```javascript
c.ne("isPublished", true);
```

### neProperty( required string property, required any otherProperty )

Where one property does not equal another

```javascript
c.neProperty("password","passwordHash");
```

### sizeEq( required string property, required any propertyValue )

Where a collection property's size equals a particular value

```javascript
c.sizeEq("comments",30);
```

### sizeGT( required string property, required any propertyValue )

Where a collection property's size is greater than a particular value

```javascript
c.sizeGT("children",5);
```

### sizeGE( required string property, required any propertyValue )

Where a collection property's size is greater than or equal to a particular value

```javascript
c.sizeGE("children", 10);
```

### sizeLT( required string property, required any propertyValue )

Where a collection property's size is less than a particular value

```javascript
c.sizeLT("childPages", 25 );
```

### sizeLE( required string property, required any propertyValue )

Where a collection property's size is less than or equal a particular value

```javascript
c.sizeLE("childPages", 25 );
```

### sizeNE( required string property, required any propertyValue )

Where a collection property's size is not equal to a particular value

```javascript
c.sizeNE("childPages",0);
```

### sql( required sql, params )

Use arbitrary SQL to modify the resultset

```javascript
c.sql("char_length( lastName ) = 10");
```

### and( Criterion, Criterion, ... )

Return the conjuction of N expressions as arguments

```javascript
c.and( c.restrictions.eq("name","luis"), c.restrictions.gt("age",30) );
```

### or( Criterion, Criterion, …. )

Return the disjunction of N expressions as arguments

```javascript
c.or( c.restrictions.eq("name","luis"), c.restrictions.eq("name", "joe") );
```

### isNot( required any criterion )

Return the negation of an expression. You can also use not()

```javascript
c.isNot( c.restrictions.eg("age", 30) );
```

### isTrue( required string property )

Returns if the property is true

```javascript
c.isTrue("isPublished");
```

{% hint style="info" %}
Adobe ColdFusion may throw an "Invalid CFML construct" error for certain CBORM methods that match [reserved operator names](https://helpx.adobe.com/coldfusion/developing-applications/the-cfml-programming-language/elements-of-cfml/reserved-words-in-coldfusion.html), such as `.and()`, `.or()`, and `.eq()`. You can use `.$and()`, `.$or()`, and `.isEq()` to avoid these errors and build cross-engine compatible code.
{% endhint %}

{% hint style="info" %}
In some cases (`isEq(), isIn(),` etc), you may receive data type mismatch errors. These can be resolved by using JavaCast on your criteria value or use our auto caster methods: `idCast(), autoCast()`
{% endhint %}

```javascript
c.isEq("userID", idCast( 3 ) );
```

You can also use the `add()` method to add a manual restriction or array of restrictions to the criteria you are building.

```javascript
c.add( c.restrictions.eq("name","luis") )
```

But as you can see from the code, the facade methods are much nicer.

## Negation

Every restriction method you see above or in the docs can also be negated very easily by just prefixing the method with a `not` .

```javascript
c
    .notEq("userID", idCast( 3 ) )
    .notBetween()
    .notIsTrue()
    .notIsFalse();
```

## when()

There are times where you need if statements in order to add criterias based on incoming input. That's ok, but we can do better by adding a `when( test, target )` function that will evaluate the `test` argument or expression. If it evaluates to true then the target closure is called for you with the criteria object so you can do your criterias:

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
