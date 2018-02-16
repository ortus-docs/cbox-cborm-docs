#Overview
![class BaseORMService](https://github.com/ColdBox/cbox-cborm/wiki/BaseORMService.jpg)

 The *BaseORMService* is a core model CFC of the module that will provide you with a tremendous gammut of API methods to interact with ColdFusion ORM Entities. 

## Concept 
The idea behind this support class is to provide a very good base or parent service layer that can interact with ColdFusion ORM via hibernate and entities inspired by [Spring's Hibernate Template](http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/classic-spring.html#classic-spring-hibernate) support.  This means that you don't need to create a service layer CFC in order to work with ORM entities. 

It provides tons of methods for query executions, paging, transactions, session metadata, caching and much more. You can either use the class on its own or create more concrete service layers by inheriting from this class. 

## Usage
In order to get started with the base ORM service you need to know how to get access to it.  You can do this via WireBox injection DSL or by the model's ID.

### WireBox DSL
The module also registers a new WireBox DSL called `entityservice` which can produce virtual or base orm entity services that you can use to inject into your own event handlers or models.

* `entityservice` - Inject a global ORM service
* `entityservice:{entityName}` - Inject a Virtual entity service according to `entityName`

### Injection

You can also request a Base ORM Service via the registered WireBox ID which is exactly the same as the `entityService` DSL:

```js
// Inject
inject name="ORMService" inject="BaseORMService@cborm";

// Retrieve
wireBox.getInstance( "BaseORMService@cborm" );
getModel( "BaseORMService@cborm" );
```

### Implementation 
Once you have access to the injected base ORM service, you can use it in all of its glory.

> **Important** Please check out the latest API Docs for the latest methods and functionality. 


```javascript
component{

  inject name="ORMService" inject="entityService";

  function saveUser( event, rc, prc ){
      // retrieve and populate a new user object
      var user = populateModel( ORMService.new( "User" ) );

      // save the entity using hibernate transactions
      ORMService.save( user );
     
      setNextEvent( "user.list" );
  }

  function list( event, rc, prc ){
    
    //get a listing of all users with paging
    prc.users = ORMService.list(
    	entityName= "User",
    	sortOrder = "fname",
    	offset 	= event.getValue("startrow",1),
    	max 		= 20
    );

    event.setView( "user/list" );
  }
}

function index( event, rc, prc ){
    prc.data = ORMService.findAll( "Permission" );
}
```

Once you have a reference to the base ORM service then you can use any of its methods to interact with ORM entities. The drawback about leveraging the base ORM model is that you cannot add custom functions to it or tell it to work on a specific entity for all operations. It is a very simple API, but if you need more control then we can start using other approaches shown below.


## Virtual Services

We also have a [virtual service layer](/virtualentityservice/Virtual Entity Service.md) that can be mapped to specific entities and create entity driven service layers virtually. Meaning you don't have to be passing any entity names to the API methods to save you precious typing time.

## Concrete Services
This is where you create your own CFC that inherits from our Base ORM Service model and either add or override methods.  You can read more about it in our [Concrete Services Section](/Concrete-Services.md) 