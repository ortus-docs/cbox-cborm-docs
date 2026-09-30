---
description: "Validation of Active Entities in CBORM using cbValidation"
icon: check-to-slot
---

# Validation

## Validation Functions

Our active entity object will also give you access to our validation engine ([cbValidation](https://coldbox-validation.ortusbooks.com/)) by giving your ORM entities the following functions:

```javascript
/**
 * Validate the ActiveEntity with the coded constraints -> this.constraints, or passed in shared or implicit constraints
 * The entity must have been populated with data before the validation
 *
 * @fields        One or more fields to validate on, by default it validates all fields in the constraints. This can be a simple list or an array.
 * @constraints   An optional shared constraints name or an actual structure of constraints to validate on.
 * @locale        An optional locale to use for i18n messages
 * @excludeFields An optional list of fields to exclude from the validation.
 * @includeFields An optional list of fields to include in the validation.
 * @profiles      An optional list of profiles to use for the validation.
 */
boolean function isValid(
	string fields        = "*",
	any constraints      = "",
	string locale        = "",
	string excludeFields = "",
	string includeFields = "",
	string profiles      = ""
)

/**
 * Get the validation results object.  This will be an empty validation object if isValid() has not being called yet.
 * The validationResult property accessor, getValidationResult(), returns the same object after isValid().
 */
cbvalidation.models.result.IValidationResult function getValidationResults()

/**
 * Validate the entity (same arguments as isValid()) and return the validation results object
 */
ValidationResult function validate( ... )

/**
 * Validate the entity (same arguments as isValid()) and throw a `ValidationException` if it fails.
 * The validation errors (JSON) are in the exception's extended information. Returns the entity back.
 */
ActiveEntity function validateOrFail( ... )
```

## Declaring Constraints

This makes it really easy for you to validate your ORM entities in two easy steps:

1\) Add your validation constraints to the entity

2\) Call the validation methods

Let's see the entity code so you can see the [constraints](https://coldbox-validation.ortusbooks.com/overview/coldbox-validation/declaring-constraints):

{% code title="models/User.bx" %}
```javascript
class persistent="true" extends="cborm.models.ActiveEntity" {

    // Properties
    property name="id" fieldtype="id" generator="uuid";
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
{% endcode %}

## Validating Constraints

Now let's check out the handlers to see how to validate the entity via the `isValid()` function:

{% code title="handlers/users.bx" %}
```javascript
class {

    property name="messagebox" inject="messagebox@cbmessagebox";

    function index(event,rc,prc){
        prc.users = getInstance( "User" ).list( sortOrder="lastName asc" );
        event.setView( "users/list" );
    }

    function save(event,rc,prc){
        event.paramValue( "id", -1 );

        var oUser = getInstance( "User" )
            .getOrFail( rc.id )
            .populate( rc );

        if( oUser.isValid() ){
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
{% endcode %}

{% hint style="info" %}
Please remember that the `isValid()` function has several arguments you can use to fine tune the validation:

* fields
* constraints
* locale
* excludeFields
* includeFields
* profiles
{% endhint %}

If you prefer exceptions, use `validateOrFail()`, which throws a `ValidationException` when the entity is not valid, or `validate()`, which returns the validation results object:

```javascript
// Throws a ValidationException if invalid, else keeps chaining
getInstance( "User" )
    .new( rc )
    .validateOrFail()
    .save();

// Get the results object back
var results = getInstance( "User" ).new( rc ).validate();
if( results.hasErrors() ){
    return results.getAllErrors();
}
```

## Displaying Errors

You can refer back to the [cbValidation](https://coldbox-validation.ortusbooks.com/overview/displaying-errors) docs for displaying errors:

{% embed url="https://coldbox-validation.ortusbooks.com/overview/displaying-errors" %}

Here are the most common methods for retreving the errors from the Result object via the `getValidationResults()` method:

* `getResultMetadata()`
* `getFieldErrors( [field] )`
* `getAllErrors( [field] )`
* `getAllErrorsAsJSON( [field] )`
* `getAllErrorsAsStruct( [field] )`
* `getErrorCount( [field] )`
* `hasErrors( [field] )`
* `getErrors()`

The [API Docs ](https://apidocs.ortussolutions.com/#/coldbox-modules/cbvalidation/)in the module (once installed) will give you the latest information about these methods and arguments.

## Unique Property Validation

We have also integrated a `UniqueValidator` from the **validation** module into our ORM module. It is mapped into WireBox as `UniqueValidator@cborm` so you can use it in your model constraints like so:

```javascript
{ username : { validator : "UniqueValidator@cborm", required : true } }
```
