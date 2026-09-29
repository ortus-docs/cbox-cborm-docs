# Unique Property Validation

We have also integrated a `UniqueValidator` from the **validation** module into our ORM module. It is mapped into WireBox as `UniqueValidator@cborm` so you can use in your model constraints like so:

```javascript
{ fieldName : { validator: "UniqueValidator@cborm" } }
```

That's it! Once you define a property with this validator, then `cbValidation` will delegate to the cborm `UniqueValidator` so it can identify if the value is unique in the database. Below you can see an example entity that marks the `userName` property as unique for validation purposes.

{% code title="models/User.cfc" %}
```javascript
component persistent="true" table="users"{

	property name="id" column="user_id" fieldType="id" generator="uuid";
	property name="firstName";
	property name="lastName";
	property name="userName" unique="true";
	property name="password";
	property name="lastLogin" ormtype="date";
	
	// M20 -> Role
	property name="role" cfc="Role" fieldtype="many-to-one" fkcolumn="FKRoleID" lazy="true" notnull="false";
	
	// DI Test
	property name="testDI" inject="model:testService" persistent="false" required="false";
	
	// Validation Constraints
	this.constraints = {
		"firstName" : { required = true },
		"lastName"  : { required = true },
		"userName"  : { required = true, validator="UniqueValidator@cborm" }
	};

}
```
{% endcode %}

Now here is a sample validation:

{% code title="handlers/users.cfc" %}
```javascript
component{
    
    property name="userService" inject="entityService:User";
    
    function create( event, rc, prc ){
        var oUser = populateModel( "User" );
        var vResults = validateModel( oUser );
        
        if( !vResults.isValid() ){
            return event
                .setHTTPHeader( 400, "Invalid Data" )
                .renderData( type="json", data=vResults.getAllErrorsAsStruct() );
        }
        
        userService.save( oUser );
        
        return oUser.getMemento();
    }


}
```
{% endcode %}

