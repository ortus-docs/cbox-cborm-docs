# Validation

Our active entity object will also give you access to our validation engine \([cbValidation](https://coldbox-validation.ortusbooks.com/)\) by giving your ORM entities the following functions:

```javascript
/**
 * Validate the ActiveEntity with the coded constraints -> this.constraints, or passed in shared or implicit constraints
 * The entity must have been populated with data before the validation
 *
 * @fields One or more fields to validate on, by default it validates all fields in the constraints. This can be a simple list or an array.
 * @constraints An optional shared constraints name or an actual structure of constraints to validate on.
 * @locale An optional locale to use for i18n messages
 * @excludeFields An optional list of fields to exclude from the validation.
 */
boolean function isValid(
	string fields="*",
	any constraints="",
	string locale="",
	string excludeFields=""
){

/**
* Get the validation results object.  This will be an empty validation object if isValid() has not being called yet.
*/
cbvalidation.models.result.IValidationResult function getValidationResults(){
```

This makes it really easy for you to validate your ORM entities in two easy steps:

1\) Add your validation constraints to the entity

2\) Call the validation methods

Let's see the entity code:

{% code-tabs %}
{% code-tabs-item title="models/User.cfc" %}
```javascript
component persistent="true" extends="cborm.models.ActiveEntity"{
    
    // Properties
    property name="firstName";
    property name="lastName";
    property name="email";
    property name="username";
    property name="password";

    // Validation Constraints
    this.constraints = {
        "firstName" = {required=true}, 
        "lastName"  = {required=true},
        "email"     = {required=true,type="email"},
        "username"  = {required=true, size="5..10"},
        "password"  = {required=true, size="5..10"}
    };
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now let's check out the handlers:

{% code-tabs %}
{% code-tabs-item title="handlers/users.cfc" %}
```javascript
component{

    property name="messagebox" inject="messagebox@cbmessagebox";

    function index(event,rc,prc){
        prc.users = getInstance( "User" ).list( sortOrder="lastName asc" );
        event.setView( "users/list" );
    }

    function save(event,rc,prc){
        event.paramValue( "id", -1 );
        
        var oUser = getInstance( "User" )
            .getOrFail( rc.id )
            .populate( rc )

        if( oUser.isValid() {
            oUser.save();
            flash.put( "notice", "User Saved!" );
            relocate( "users.index" );
        }
        else{
            messagebox.error( messageArray=oUser.getValidationResults().getAllErrors() );
            editor( event, rc, prc );
        }

    }

    function editor(event,rc,prc){
        event.paramValue( "id", -1 );
        prc.user = getInstance( "User" ).getOrFail( rc.id );
        event.setView( "users/editor" );
    }

    function delete(event,rc,prc){
        event.paramValue( "id", -1 );
        getInstance( "User" )
            .deleteById( rc.id );
        flash.put( "notice", "User Removed!" );
        relocate( "users.index" );
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



