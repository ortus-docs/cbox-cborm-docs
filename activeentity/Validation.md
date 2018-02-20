#Validation
Our active entity object will also give you access to our validation engine by giving your ORM entities the following functions:

```javascript
/**
* Validate the ActiveEntity with the coded constraints -> this.constraints, or passed in shared or implicit constraints
* The entity must have been populated with data before the validation
* @fields.hint One or more fields to validate on, by default it validates all fields in the constraints. This can be a simple list or an array.
* @constraints.hint An optional shared constraints name or an actual structure of constraints to validate on.
* @locale.hint An optional locale to use for i18n messages
* @excludeFields.hint An optional list of fields to exclude from the validation.
*/
boolean function isValid(string fields="*", any constraints="", string locale="", string excludeFields="");

/**
* Get the validation results object.  This will be an empty validation object if isValid() has not being called yet.
*/
coldbox.system.validation.result.IValidationResult function getValidationResults();
```
> **Important** In order for validation to work, WireBox ORM entity injection must be enabled first! 
> * ColdBox ORM Entity Injection
> * WireBox Standalone ORM Entity Injection

This makes it really easy for you to validate your ORM entities.

Sample:

```javascript
import coldbox.system.orm.hibernate.*;
component persistent="true" extends="ActiveEntity"{
	// Properties
	property name="firstName";
	property name="lastName";
	property name="email";
	property name="username";
	property name="password";

	// Validation Constraints
	this.constraints = {
		"firstName" = {required=true}, 
		"lastName" = {required=true},
		"email" = {required=true,type="email"},
		"username" = {required=true, size="5..10"},
		"password" = {required=true, size="5..10"}
	};
}
```

Handlers Sample: 

```javascript
component{
	
        property name="Messagebox" inject="id:messagebox@cbmessagebox";

	function index(event,rc,prc){
		prc.users = entityNew("User").list(sortOrder="lastName asc");
		event.setView("users/list");
	}

	function save(event,rc,prc){
		event.paramValue("id","");
		var user = populateModel( entityNew("User").get( rc.id ) );

		if( user.isValid() {
			user.save();
			flash.put("notice","User Saved!");
			setNextEvent("users.index");
		}
		else{
			Messagebox.error(messageArray=user.getValidationResults().getAllErrors());
			editor(event,rc,prc);
		}

	}

	function editor(event,rc,prc){
		event.paramValue("id","");
		prc.user = entityNew("User").get( rc.id );
		event.setView("users/editor");
	}

	function delete(event,rc,prc){
		event.paramValue("id","");
		entityNew("User").deleteById( rc.id );
		flash.put("notice","User Removed!");
		setNextEvent("users.index");
	}
}
```

Please refer to the ORM:BaseORMService for all the cool methods and functionality you can use with Active Entity. Like always, refer to the latest CFC Docs for methods and arguments.