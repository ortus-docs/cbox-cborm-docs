---
description: Functional helper methods for building expressive, chainable ActiveEntity operations
icon: link
---

# Functional Helpers

ActiveEntity provides several functional helper methods that enable cleaner, more expressive entity building without breaking the fluent chain or requiring intermediate variables. These methods were introduced in CBORM 4.6.0 and allow you to build entities using functional programming patterns.

## Overview

The functional helpers provide flow control capabilities directly within your entity chains:

* **`peek()`** - Inspect or log the entity during building without breaking the chain
* **`when()`** - Conditionally execute logic based on a boolean expression
* **`unless()`** - Execute logic when a condition is false (opposite of `when()`)
* **`throwIf()`** - Throw an exception if a condition is met
* **`throwUnless()`** - Throw an exception unless a condition is met

All functional helpers return the entity itself, enabling continuous method chaining.

## peek()

The `peek()` method allows you to inspect, log, or interact with the entity during the building process without breaking the chain. This is particularly useful for debugging or auditing entity state at specific points.

### Signature

```javascript
any function peek( required target )
```

### Arguments

| Argument | Type     | Required | Description                                            |
| -------- | -------- | -------- | ------------------------------------------------------ |
| target   | closure  | Yes      | A closure that receives the entity as its argument     |

### Examples

{% tabs %}

{% tab title="BoxLang" %}
```groovy
// Debug entity state during building
user = new User()
    .setFirstName( "Luis" )
    .setLastName( "Majano" )
    .peek( ( entity ) => {
        // Log the current state
        systemOutput( "Building user: #entity.getFirstName()# #entity.getLastName()#" )
    } )
    .setEmail( "lmajano@ortussolutions.com" )
    .save()

// Dump memento for inspection
user = new User()
    .populate( rc )
    .peek( ( entity ) => writeDump( entity.getMemento() ) )
    .save()

// Conditional logging
user = new User()
    .setAge( rc.age )
    .peek( ( entity ) => {
        if ( entity.getAge() < 18 ) {
            systemOutput( "Minor user detected: #entity.getAge()#" )
        }
    } )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Debug entity state during building
user = new User()
    .setFirstName( "Luis" )
    .setLastName( "Majano" )
    .peek( function( entity ) {
        // Log the current state
        systemOutput( "Building user: #entity.getFirstName()# #entity.getLastName()#" );
    } )
    .setEmail( "lmajano@ortussolutions.com" )
    .save();

// Dump memento for inspection
user = new User()
    .populate( rc )
    .peek( function( entity ) {
        writeDump( entity.getMemento() );
    } )
    .save();

// Conditional logging
user = new User()
    .setAge( rc.age )
    .peek( function( entity ) {
        if ( entity.getAge() < 18 ) {
            systemOutput( "Minor user detected: #entity.getAge()#" );
        }
    } )
    .save();
```
{% endtab %}

{% endtabs %}

## when()

The `when()` method provides conditional execution within your entity chain. If the boolean expression evaluates to `true`, the success closure executes; otherwise, the optional failure closure executes.

### Signature

```javascript
any function when( required boolean target, required success, failure )
```

### Arguments

| Argument | Type     | Required | Description                                           |
| -------- | -------- | -------- | ----------------------------------------------------- |
| target   | boolean  | Yes      | The boolean expression to evaluate                    |
| success  | closure  | Yes      | Closure to execute if target is true                  |
| failure  | closure  | No       | Closure to execute if target is false                 |

### Examples

{% tabs %}

{% tab title="BoxLang" %}
```groovy
// Set role based on admin flag
user = new User()
    .setEmail( rc.email )
    .when(
        rc.isAdmin,
        ( entity ) => entity.setRole( "administrator" ),
        ( entity ) => entity.setRole( "user" )
    )
    .save()

// Apply discount for premium members
order = new Order()
    .setTotal( calculateTotal( rc.items ) )
    .when(
        user.isPremiumMember(),
        ( entity ) => entity.applyDiscount( 0.15 )
    )
    .save()

// Conditional validation
user = new User()
    .populate( rc )
    .when(
        rc.requiresVerification,
        ( entity ) => entity.setVerificationToken( createUUID() )
    )
    .save()

// Multiple conditions
post = new Post()
    .setContent( rc.content )
    .when(
        !isNull( rc.categoryID ),
        ( entity ) => entity.setCategory( categoryService.get( rc.categoryID ) )
    )
    .when(
        rc.publish ?: false,
        ( entity ) => entity.setPublishedDate( now() )
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Set role based on admin flag
user = new User()
    .setEmail( rc.email )
    .when(
        rc.isAdmin,
        function( entity ) { entity.setRole( "administrator" ); },
        function( entity ) { entity.setRole( "user" ); }
    )
    .save();

// Apply discount for premium members
order = new Order()
    .setTotal( calculateTotal( rc.items ) )
    .when(
        user.isPremiumMember(),
        function( entity ) { entity.applyDiscount( 0.15 ); }
    )
    .save();

// Conditional validation
user = new User()
    .populate( rc )
    .when(
        rc.requiresVerification,
        function( entity ) { entity.setVerificationToken( createUUID() ); }
    )
    .save();

// Multiple conditions
post = new Post()
    .setContent( rc.content )
    .when(
        !isNull( rc.categoryID ),
        function( entity ) { entity.setCategory( categoryService.get( rc.categoryID ) ); }
    )
    .when(
        rc.publish ?: false,
        function( entity ) { entity.setPublishedDate( now() ); }
    )
    .save();
```
{% endtab %}

{% endtabs %}

## unless()

The `unless()` method is the logical opposite of `when()`. It executes the success closure when the condition is `false`, and optionally executes the failure closure when the condition is `true`.

### Signature

```javascript
any function unless( required boolean target, required success, failure )
```

### Arguments

| Argument | Type     | Required | Description                                           |
| -------- | -------- | -------- | ----------------------------------------------------- |
| target   | boolean  | Yes      | The boolean expression to evaluate                    |
| success  | closure  | Yes      | Closure to execute if target is false                 |
| failure  | closure  | No       | Closure to execute if target is true                  |

### Examples

{% tabs %}

{% tab title="BoxLang" %}
```groovy
// Deactivate user unless email is verified
user = new User()
    .populate( rc )
    .setActive( true )
    .unless(
        user.hasVerifiedEmail(),
        ( entity ) => entity.setActive( false )
    )
    .save()

// Send welcome email unless already sent
user = new User()
    .populate( rc )
    .unless(
        user.getWelcomeEmailSent(),
        ( entity ) => mailService.sendWelcome( entity.getEmail() )
    )
    .save()

// Apply default category unless specified
post = new Post()
    .populate( rc )
    .unless(
        !isNull( rc.categoryID ),
        ( entity ) => entity.setCategory( categoryService.getDefault() )
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Deactivate user unless email is verified
user = new User()
    .populate( rc )
    .setActive( true )
    .unless(
        user.hasVerifiedEmail(),
        function( entity ) { entity.setActive( false ); }
    )
    .save();

// Send welcome email unless already sent
user = new User()
    .populate( rc )
    .unless(
        user.getWelcomeEmailSent(),
        function( entity ) { mailService.sendWelcome( entity.getEmail() ); }
    )
    .save();

// Apply default category unless specified
post = new Post()
    .populate( rc )
    .unless(
        !isNull( rc.categoryID ),
        function( entity ) { entity.setCategory( categoryService.getDefault() ); }
    )
    .save();
```
{% endtab %}

{% endtabs %}

## throwIf()

The `throwIf()` method throws a controlled exception if the boolean condition evaluates to `true`. This is useful for inline validation and early failure detection in your entity chains.

### Signature

```javascript
any function throwIf( required boolean target, required type, message = "", detail = "" )
```

### Arguments

| Argument | Type     | Required | Default | Description                                      |
| -------- | -------- | -------- | ------- | ------------------------------------------------ |
| target   | boolean  | Yes      |         | The boolean expression to evaluate               |
| type     | string   | Yes      |         | The exception type to throw                      |
| message  | string   | No       | ""      | The exception message                            |
| detail   | string   | No       | ""      | Additional exception details                     |

### Examples

{% tabs %}

{% tab title="BoxLang" %}
```groovy
// Validate age requirement
user = new User()
    .populate( rc )
    .throwIf(
        user.getAge() < 18,
        "ValidationException",
        "User must be 18 or older",
        "Age provided: #user.getAge()#"
    )
    .save()

// Enforce business rules
order = new Order()
    .populate( rc )
    .throwIf(
        order.getTotal() > user.getCreditLimit(),
        "InsufficientCreditException",
        "Order total exceeds credit limit"
    )
    .save()

// Validate required relationships
post = new Post()
    .populate( rc )
    .throwIf(
        isNull( post.getAuthor() ),
        "ValidationException",
        "Post must have an author"
    )
    .save()

// Multiple validations
user = new User()
    .populate( rc )
    .throwIf(
        user.getUsername().len() < 3,
        "ValidationException",
        "Username must be at least 3 characters"
    )
    .throwIf(
        !isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email address"
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Validate age requirement
user = new User()
    .populate( rc )
    .throwIf(
        user.getAge() < 18,
        "ValidationException",
        "User must be 18 or older",
        "Age provided: #user.getAge()#"
    )
    .save();

// Enforce business rules
order = new Order()
    .populate( rc )
    .throwIf(
        order.getTotal() > user.getCreditLimit(),
        "InsufficientCreditException",
        "Order total exceeds credit limit"
    )
    .save();

// Validate required relationships
post = new Post()
    .populate( rc )
    .throwIf(
        isNull( post.getAuthor() ),
        "ValidationException",
        "Post must have an author"
    )
    .save();

// Multiple validations
user = new User()
    .populate( rc )
    .throwIf(
        user.getUsername().len() < 3,
        "ValidationException",
        "Username must be at least 3 characters"
    )
    .throwIf(
        !isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email address"
    )
    .save();
```
{% endtab %}

{% endtabs %}

## throwUnless()

The `throwUnless()` method is the logical opposite of `throwIf()`. It throws a controlled exception if the boolean condition evaluates to `false`.

### Signature

```javascript
any function throwUnless( required boolean target, required type, message = "", detail = "" )
```

### Arguments

| Argument | Type     | Required | Default | Description                                      |
| -------- | -------- | -------- | ------- | ------------------------------------------------ |
| target   | boolean  | Yes      |         | The boolean expression to evaluate               |
| type     | string   | Yes      |         | The exception type to throw                      |
| message  | string   | No       | ""      | The exception message                            |
| detail   | string   | No       | ""      | Additional exception details                     |

### Examples

{% tabs %}

{% tab title="BoxLang" %}
```groovy
// Require email validation
user = new User()
    .populate( rc )
    .throwUnless(
        isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email address format"
    )
    .save()

// Ensure user has required permissions
post = new Post()
    .populate( rc )
    .throwUnless(
        user.hasPermission( "publish" ),
        "SecurityException",
        "User does not have permission to publish posts"
    )
    .save()

// Validate entity state
order = new Order()
    .populate( rc )
    .throwUnless(
        order.getItems().len() > 0,
        "ValidationException",
        "Order must contain at least one item"
    )
    .save()

// Check required fields
user = new User()
    .populate( rc )
    .throwUnless(
        !isNull( user.getEmail() ) && user.getEmail().len() > 0,
        "ValidationException",
        "Email is required"
    )
    .throwUnless(
        !isNull( user.getUsername() ) && user.getUsername().len() > 0,
        "ValidationException",
        "Username is required"
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Require email validation
user = new User()
    .populate( rc )
    .throwUnless(
        isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email address format"
    )
    .save();

// Ensure user has required permissions
post = new Post()
    .populate( rc )
    .throwUnless(
        user.hasPermission( "publish" ),
        "SecurityException",
        "User does not have permission to publish posts"
    )
    .save();

// Validate entity state
order = new Order()
    .populate( rc )
    .throwUnless(
        order.getItems().len() > 0,
        "ValidationException",
        "Order must contain at least one item"
    )
    .save();

// Check required fields
user = new User()
    .populate( rc )
    .throwUnless(
        !isNull( user.getEmail() ) && user.getEmail().len() > 0,
        "ValidationException",
        "Email is required"
    )
    .throwUnless(
        !isNull( user.getUsername() ) && user.getUsername().len() > 0,
        "ValidationException",
        "Username is required"
    )
    .save();
```
{% endtab %}

{% endtabs %}

## Best Practices

### Chaining Multiple Helpers

You can chain multiple functional helpers together to build complex entity logic:

{% tabs %}

{% tab title="BoxLang" %}
```groovy
user = new User()
    .populate( rc )
    .peek( ( entity ) => systemOutput( "Processing user: #entity.getEmail()#" ) )
    .throwUnless(
        isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email"
    )
    .when(
        rc.isAdmin,
        ( entity ) => entity.setRole( "administrator" )
    )
    .unless(
        user.hasVerifiedEmail(),
        ( entity ) => entity.setActive( false )
    )
    .throwIf(
        user.getAge() < 18,
        "ValidationException",
        "Must be 18 or older"
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
user = new User()
    .populate( rc )
    .peek( function( entity ) {
        systemOutput( "Processing user: #entity.getEmail()#" );
    } )
    .throwUnless(
        isValid( "email", user.getEmail() ),
        "ValidationException",
        "Invalid email"
    )
    .when(
        rc.isAdmin,
        function( entity ) { entity.setRole( "administrator" ); }
    )
    .unless(
        user.hasVerifiedEmail(),
        function( entity ) { entity.setActive( false ); }
    )
    .throwIf(
        user.getAge() < 18,
        "ValidationException",
        "Must be 18 or older"
    )
    .save();
```
{% endtab %}

{% endtabs %}

### Use with CBValidation

Functional helpers work well alongside [cbvalidation](../active-record/validation.md) for comprehensive entity validation:

{% tabs %}

{% tab title="BoxLang" %}
```groovy
user = new User()
    .populate( rc )
    .throwUnless(
        user.isValid(),
        "ValidationException",
        "Validation failed",
        user.getValidationResults().getAllErrorsAsJson()
    )
    .when(
        user.isNewEntity(),
        ( entity ) => mailService.sendWelcomeEmail( entity.getEmail() )
    )
    .save()
```
{% endtab %}

{% tab title="CFML" %}
```javascript
user = new User()
    .populate( rc )
    .throwUnless(
        user.isValid(),
        "ValidationException",
        "Validation failed",
        user.getValidationResults().getAllErrorsAsJson()
    )
    .when(
        user.isNewEntity(),
        function( entity ) {
            mailService.sendWelcomeEmail( entity.getEmail() );
        }
    )
    .save();
```
{% endtab %}

{% endtabs %}

### Performance Considerations

* **Avoid complex logic**: Keep closures simple and focused on single responsibilities
* **Use peek() for debugging**: Remove or comment out `peek()` calls in production for better performance
* **Prefer early validation**: Use `throwIf()` and `throwUnless()` early in the chain to fail fast

### Readability Tips

* **One condition per helper**: Don't nest multiple conditions in a single `when()` or `unless()` call
* **Descriptive exception messages**: Use clear, actionable messages in `throwIf()` and `throwUnless()`
* **Consistent formatting**: Format chains consistently for better readability

{% hint style="success" %}
These functional helpers enable cleaner, more expressive entity building without breaking the fluent chain or requiring intermediate variables. They promote functional programming patterns and make entity logic more declarative and easier to test.
{% endhint %}

## Related Topics

* [Active Entity Overview](active-entity.md)
* [Usage](usage.md)
* [Validation](validation.md)
* [BaseORMService when() Method](../base-orm-service/service-methods/utility-methods/when.md)
