# What's New With 2.2.0

This release not only has some bug fixes but several new features that pack a punch.

## Major Features

### Criteria Query Fluent If Statements - `when()`

How many times have you been dealing with if statements in order to add some restrictions into your criteria object? Many, this was the only way before, not anymore.  So instead of doing something like the following:

```javascript
c = newCriteria();

if( isBoolean( arguments.isPublished ) ){
  
	c.isEq( "isPublished", isPublished );

	// Published eq true evaluate other params
    if( isPublished ){
        c.isLt( "publishedDate", now() )
        .$or( c.restrictions.isNull( "expireDate" ), c.restrictions.isGT( "expireDate", now() ) )
        .isEq( "passwordProtection","" );
    }

}

if( !isNull( arguments.showInSearch ) ){
	c.isEq( "showInSearch", showInSearch );
}


return c.list();
```

This looks like normal code, but we can do a more functional approach by introducing the `when()` function:

```javascript
/**
* @test The boolean evaluation
* @target The closure to execute if test is true
*/
when( boolean test, function target )
```

This function takes in as the first argument a **boolean** value, if the value is **true**, then the target closure will be called for you and the criteria will be passed via the arguments scope:

```javascript
newCriteria()
    .when( isBoolean( arguments.isPublished ), function( c ){
        // Published bit
        c.isEq( "isPublished", isPublished )
        	.when( isPublished, function( c ){
        		c.isLt( "publishedDate", now() )
	            .$or( c.restrictions.isNull( "expireDate" ), c.restrictions.isGT( "expireDate", now() ) )
	            .isEq( "passwordProtection","" );
        	} )
    } )
  .when( !isNull( arguments.showInSearch ), function( criteria ){	
  	c.isEq( "showInSearch", showInSearch );
   } )
  .list()
```

This construct will help you create more fluent designs when building criteria queries, enjoy!

### PeekaBoo! - `peek()`

We have also enhanced the criteria queries with a peek\(\) function which allows you to peek in the current position of the criteria build up.  This allows you to debug or inspect the SQL/HQL inside the criteria at that point in time.  You can use it for sending debug data or logging, or auditing.

```javascript
/**
* @target the closure to execute, the criteria is passed as an argument
*/
peek( target )
```

Enjoy your peekaboo function!

```javascript
newCriteria()
    .when( isBoolean( arguments.isPublished ), function( c ){
        // Published bit
        c.isEq( "isPublished", isPublished )
        	.when( isPublished, function( c ){
        		c.isLt( "publishedDate", now() )
	            .$or( c.restrictions.isNull( "expireDate" ), c.restrictions.isGT( "expireDate", now() ) )
	            .isEq( "passwordProtection","" );
        	} )
    } )
  .peek( function( c ){
      systemOutput( "HQL after publish: #criteria.getSql()# ");
  } )
  .when( !isNull( arguments.showInSearch ), function( criteria ){	
  	c.isEq( "showInSearch", showInSearch );
   } )
   .peek( function( c ){
      systemOutput( "HQL before listing: #criteria.getSql()# ");
  } )
  .list()
```

### ValidateOrFail\(\)

We have added a new function on the ActiveEntity object to assist with validations. The `validateOrFail()` function will allow you to validate the entity and if it validates it just returns the instance of the entity for a nice fluent design.  However, if the validation fails, it throws a `ValidationException` and the errors are passed to the exception object via the `extendedInfo` key.  You can then deal with the exception as needed.

```javascript
getInstance( "User" )
    .populate( rc )
    .validateOrFail()
    .save();
```

## Release Notes

* `Features`: New function for criteria query `when( boolean, target )` that you can use to build functional criterias without the use of if statements.
* `Feature`: Missing `nullValue()` is BaseBuilder class
* `Feature`: Added new criteria query `peek( closure )` function to allow for peeking into the building process. Pass in your closure that receives the criteria and interact with it.
* `Feature`: Added a `validateOrFail()` to the active entity, which if the validation fails it will throw an exception or return back to you the same entity validated now.
* `Improvement`: Better documentation for `deleteById()` since it does bulk deletion, which does not do any type of cascading.
* `Improvement`: `isValid()` in active entity missing `includeFields` argument
* `Improvement`: Timeout hints for criteria builder
* `Improvement`: Updated exception type for criteria builder `get()`
* `Bug`: ACF2016 issues with elvis operator.
* `Bug`: `getOrFail()` had an invalid throw statement

