# Basic Crud - Services

Let's do a basic example of how to work with **cborm** when doing basic CRUD \(Create-Read-Update-Delete\).  We will generate a ColdBox App, connect it to a database and leverage **a** virtual service layer for a nice quick CRUD App.

The source code for this full example can be found in Github: [https://github.com/coldbox-samples/cborm-crud-demo](https://github.com/coldbox-samples/cborm-crud-demo) or in ForgeBox: [https://forgebox.io/view/cborm-crud-demo](https://forgebox.io/view/cborm-crud-demo)

## ColdBox App

Let's start by creating a ColdBox app and preparing it for usage with ORM:

```bash
# Create folder
mkdir myapp --cd
# Scaffold App
coldbox create app
# Install cborm, dotenv, and cfconfig so we can get the CFML engine talking to the DB fast.
install cborm,commandbox-dotenv,commandbox-cfconfig
# Update the .env file
cp .env.example .env
```

### Setup Environment

Season the environment file \(`.env`\) with your database credentials and make sure that database exists:

```bash
# ColdBox Environment
APPNAME=ColdBox
ENVIRONMENT=development

# Database Information
DB_CONNECTIONSTRING=jdbc:mysql://127.0.0.1:3306/cborm?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC&useLegacyDatetimeCode=true
DB_CLASS=com.mysql.jdbc.Driver
DB_DRIVER=MySQL
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=cborm
DB_USER=root
DB_PASSWORD=cborm

# S3 Information
S3_ACCESS_KEY=
S3_SECRET_KEY=
S3_REGION=us-east-1
S3_DOMAIN=amazonaws.com
```

### Setup ORM

Now open the `Application.cfc` and let's configure the ORM by adding the following in the pseudo constructor and adding two lines of code to the request start so when we reinit the APP we can also reinit the ORM.

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
```javascript
// Locate the cborm module for events
this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";

// The default dsn name in the ColdBox scaffold
this.datasource = "coldbox"; 
// ORM Settings + Datasource
this.ormEnabled = "true";
this.ormSettings = {
	cfclocation = [ "models" ], // Where our entities exist
	logSQL = true, // Remove after development to false.
	dbcreate = "update", // Generate our DB
	automanageSession = false, // Let cborm manage it
	flushAtRequestEnd = false, // Never do this! Let cborm manage it
	eventhandling = true, // Enable events
	eventHandler = "cborm.models.EventHandler", // Who handles the events
	skipcfcWithError = true // Yes, because we must work in all CFML engines
};

// request start
public boolean function onRequestStart( string targetPage ){
	// If we reinit our app, reinit the ORM too
	if( application.cbBootstrap.isFWReinit() )
		ormReload();
	
	// Process ColdBox Request
	application.cbBootstrap.onRequestStart( arguments.targetPage );

	return true;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
To change the datasource name to something you like then update it here and in the `.cfconfig.json` file.  Once done, issue a `server restart` and enjoy your new datasource name.
{% endhint %}

### Start Server

Let's start a server and start enjoying the fruits of our labor:

```bash
# Start a default Lucee Server
server start
```

{% hint style="danger" %}
If you get a `Could not instantiate connection provider: org.lucee.extension.orm.hibernate.jdbc.ConnectionProviderImpl` error on startup here. It means that you hit the stupid Lucee bug where on first server start the ORM is not fully deployed.  Just issue a `server restart` to resolve this.
{% endhint %}

## Create Entity - Person.cfc

Let's start by creating a **Person** object with a few properties, let's use CommandBox for this and our super duper `coldbox create orm-entity` command:

```bash
coldbox create orm-entity 
    entityName="Person"
    properties=name,age:integer,lastVisit:timestamp
```

This will generate the `models/Person.cfc` as an `ActiveEntity` object and even create the unit test for it.

{% code-tabs %}
{% code-tabs-item title="Person.cfc" %}
```javascript
/**
 * A cool Person entity
 */
component persistent="true" table="Person"{

	// Primary Key
	property name="id" fieldtype="id" column="id" generator="native" setter="false";
	
	// Properties
	property name="name" ormtype="string";
	property name="age" ormtype="numeric";
	property name="lastVisit" ormtype="timestamp";
	
	// Validation
	this.constraints = {
		// Example: age = { required=true, min="18", type="numeric" }
	};
	
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Setup for BDD

Since we love to promote tests at Ortus, let's configure our test harness for ORM testing. Open the `/tests/Application.cfc` and add the following code to setup the ORM and some functions for helping us test.

{% code-tabs %}
{% code-tabs-item title="/tests/Application.cfc" %}
```javascript
	// Locate the cborm module for events
	this.mappings[ "/cborm" ] = rootPath & "modules/cborm";

	// ORM Settings + Datasource
	this.datasource = "coldbox"; // The default dsn name in the ColdBox scaffold
	this.ormEnabled = "true";
	this.ormSettings = {
		cfclocation = [ "models" ], // Where our entities exist
		logSQL = true, // Remove after development to false.
		dbcreate = "update", // Generate our DB
		automanageSession = false, // Let cborm manage it
		flushAtRequestEnd = false, // Never do this! Let cborm manage it
		eventhandling = true, // Enable events
		eventHandler = "cborm.models.EventHandler", // Who handles the events
		skipcfcWithError = true // Yes, because we must work in all CFML engines
	};

	public boolean function onRequestStart( string targetPage ){
		ormReload();
		return true;
	}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now that we have prepared the test harness for ORM testing, let's test out our Person with a simple unit test. We don't over test here because our integration test will be more pragmatic and cover our use cases:

{% code-tabs %}
{% code-tabs-item title="/tests/specs/unit/PersonTest.cfc" %}
```javascript
component extends="coldbox.system.testing.BaseTestCase"{

	function run(){
		describe( "Person", function(){
			it( "can be created", function(){
				expect( getInstance( "Person" ) ).toBeComponent()
			});
		});
	}

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Basic CRUD

We will now generate a handler and do CRUD actions for this Person:

```bash
coldbox create handler 
    name="persons" 
    actions="index,create,show,update,delete" 
    views=false
```

This creates the `handlers/persons.cfc` with the CRUD actions and a nice `index` action we will use to present all persons just for fun!  

{% hint style="success" %}
Please note that this also generates the integrations tests as well under `/tests/specs/integration/personsTest.cfc`
{% endhint %}

### Inject Service

Open the `handlers/persons.cfc` and in the pseudo-constructor let's inject a virtual ORM service layer based on the `Person` entity:

{% code-tabs %}
{% code-tabs-item title="/handlers/persons.cfc" %}
```javascript
/**
 * I manage Persons
 */
component{
 
 // Inject our service layer
 property name="personService" inject="entityService:Person";
 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The `cborm` module gives you the `entityService:{entityName}` DSL which allows you to inject virtual service layers according to `entityName`.  With our code above we will have a `personService` in our `variables` scope injected for us.

### Create

We will get an instance of a Person, populate it with data and save it. We will then return it as a json memento. The `new()` method will allow you to pass a struct of properties and/or relationships to populate the new Person instance with.  Then just call the `save()` operation on the returned object.

```javascript
/**
 * create a person
 */
function create( event, rc, prc ){
	prc.person = personService
		.new( {
			name 	: "Luis",
			age 	: 40,
			lastVisit : now()
		} );
	;
	return personService
		.save( prc.person )
		.getMemento( includes="id" );
}
```

You might be asking yourself: Where does this magic `getMemento()` method come from? Well, it comes from the [mementifier](https://forgebox.io/view/mementifier) module which inspects ORM entities and injects them with this function to allow you to produce raw state from entities. \(Please see: [https://forgebox.io/view/mementifier](https://forgebox.io/view/mementifier)\)

### Read

We will get an instance according to ID and show it's memento in json. There are many ways in the ORM service and Active Entity to get objects by criteria, 

```javascript
/**
 * show a person
 */
function show( event, rc, prc ){
	return personService
		.get( rc.id ?: 0 )
		.getMemento( includes="id" );
}
```

In this example, we use the `get()` method which retrieves a single entity by identifier.  Also note the default value of `0` used as well. This means that if the incoming id is null then pass a `0`.  The orm services will detect the `0` and by default give you a **new** Person object, the call will not fail.  If you want your call to fail so you can show a nice exception for invalid identifiers you can use `getOrFail()` instead.

```javascript
/**
 * show a person
 */
function show( event, rc, prc ){
	return personService
		.getOrFail( rc.id ?: -1 )
		.getMemento( includes="id" );
}
```

### Update

Now let's retrieve an entity by Id, update it and save it again!

```javascript
/**
 * Update a person
 */
function update( event, rc, prc ){
	prc.person = personService
		.getOrFail( rc.id ?: -1 )
		.setName( "Bob" )
	return personService
		.save( prc.person )
		.getMemento( includes="id" );
}
```

### Delete

Now let's delete an incoming entity identifier

```javascript
/**
 * Delete a Person
 */
function delete( event, rc, prc ){
	try{
		personService
			.getOrFail( rc.id ?: '' )
			.delete();
		// Or use the shorthnd notation which is faster
		// getIntance( "Person" ).deleteById( rc.id ?: '' )
	} catch( any e ){
		return "Error deleting entity: #e.message# #e.detail#";
	}

	return "Entity Deleted!";
}
```

Note that you have two choices when deleting by identifier:

1. Get the entity by the ID and then send it to be deleted
2. Use the `deleteById()` and pass in the identifier

The latter allows you to bypass any entity loading, and do a pure HQL delete of the entity via it's identifier.  The first option is more resource intensive as it has to do a 1+ SQL calls to load the entity and then a final SQL call to delete it.

### List All

For extra credit, we will get all instances of `Person` and render their memento's

```javascript
/**
 * List all Persons
 */
function index( event, rc, prc ){
	return personService
		// List all as array of objects
		.list( asQuery=false )
		// Map the entities to mementos
		.map( function( item ){
			return item.getMemento( includes="id" );
		} );
}
```

That's it! We are now rolling with basic CRUD `cborm` style!

### BDD Tests

Here are the full completed BDD tests as well

{% code-tabs %}
{% code-tabs-item title="/tests/specs/integration/personsTest.cfc" %}
```javascript
component extends="coldbox.system.testing.BaseTestCase" appMapping="/"{

	function run(){

		describe( "persons Suite", function(){

			aroundEach( function( spec ) {
				setup();
				transaction{
					try{
						arguments.spec.body();
					} catch( any e ){
						rethrow;
					} finally{
						transactionRollback();
					}
				}
		   	});

			it( "index", function(){
				var event = this.GET( "persons.index" );
				// expectations go here.
				expect( event.getRenderedContent() ).toBeJSON();
			});

			it( "create", function(){
				var event = this.POST(
					"persons.create"
				);
				// expectations go here.
				var person = event.getPrivateValue( "Person" );
				expect( person ).toBeComponent();
				expect( person.getId() ).notToBeNull();
			});

			it( "show", function(){
				// Create mock
				var event = this.POST(
					"persons.create"
				);
				// Retrieve it
				var event = this.GET(
					"persons.show", {
						id : event.getPrivateValue( "Person" ).getId()
					}
				);
				// expectations go here.
				var person = event.getPrivateValue( "Person" );
				expect( person ).toBeComponent();
				expect( person.getId() ).notToBeNull();
			});

			it( "update", function(){
				// Create mock
				var event = this.POST(
					"persons.create"
				);
				var event = this.POST(
					"persons.update", {
						id : event.getPrivateValue( "Person" ).getId()
					}
				);
				// expectations go here.
				var person = event.getPrivateValue( "Person" );
				expect( person ).toBeComponent();
				expect( person.getId() ).notToBeNull();
				expect( person.getName() ).toBe( "Bob" );
			});

			it( "delete", function(){
				// Create mock
				var event = this.POST(
					"persons.create"
				);
				// Create mock
				var event = this.DELETE(
					"persons.delete", {
						id : event.getPrivateValue( "Person" ).getId()
					}
				);
				expect( event.getRenderedContent() ).toInclude( "Entity Deleted" );
			});

		});

	}

}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

