# Basic Crud

Let's do a basic example of how to work the **cborm** when doing basic CRUD \(Create-Read-Update-Delete\).  We won't go into all fancy areas, just the basics of enhanced **cborm** functionality for CRUD.  We will generate a ColdBox App, connect it to a database and leverage **ActiveEntity** for a nice quick CRUD App.  

{% hint style="info" %}
For extra credit, convert the app to NOT use **ActiveEntity** but just Virtual Entity Services.
{% endhint %}

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

### .env

Season the environment file with your database credentials and make sure that database exists:

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

### Application.cfc

Now open the `Application.cfc` and let's configure the ORM by adding the following in the pseudo constructor and adding two lines of code to the request start so when we reinit the APP we can also reinit the ORM.

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
```javascript
// Locate the cborm module for events
this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";

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

### Start Server

Let's start a server and start enjoying the fruits of our labor:

```bash
# Start a default Lucee Server
server start
```

{% hint style="danger" %}
If you get a `Could not instantiate connection provider: org.lucee.extension.orm.hibernate.jdbc.ConnectionProviderImpl` error on startup here. It means that you hit the stupid Lucee bug where on first server start the ORM is not fully deployed.  Just issue a `server restart` to resolve this.
{% endhint %}

## Entity - Person.cfc

Let's start by creating a Person object with a few properties, let's use CommandBox for this and our super duper `coldbox create orm-entity` command:

```bash
coldbox create orm-entity entityName="Person"
activeEntity=true
properties=name,age:numeric,lastVisit:timestamp
```

This will generate the `models/Person.cfc` as an `ActiveEntity` object and even create the unit test for it.

{% code-tabs %}
{% code-tabs-item title="Person.cfc" %}
```javascript
/**
 * A cool Person entity
 */
component persistent="true" table="Person" extends="cborm.models.ActiveEntity"{

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
	
	// Constructor
	function init(){
		super.init( useQueryCaching="false" );
		return this;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Basic CRUD

We will now generate a handler and do CRUD actions for this Person

```bash
coldbox create handler 
name="persons" 
actions="index,create,show,update,delete" 
views=false
```

This creates the `handlers/persons.cfc` with the CRUD actions and a nice index action we will use to present all persons just for fun!  Please note that this also generates the integrations tests as well! Let's build it out.

#### Create



#### Read



#### Update



#### Delete



#### List All



