---
description: What's new in CBOrm 4.6.0
---

# What's New With 4.6.0

CBOrm 4.6.0 brings important bug fixes for multi-datasource support and introduces powerful new functional helpers for ActiveEntity.

## Fixed

### Multi-Datasource Event Handling

* **[CBORM-37](https://ortussolutions.atlassian.net/browse/CBORM-37)**: Fixed critical issue in the `PostLoad` event handler that affected entities using non-default datasources. Multi-datasource applications now properly fire post-load events.

### Platform Support Cleanup

* **Deprecated Engine Removal**: Removed support for deprecated CFML engine versions that have reached end-of-life

## Added

### BoxLang Testing

* **BoxLang Auto-Testing**: Implemented automated test suite execution for BoxLang runtime

### ActiveEntity Functional Helpers

Several new flow control methods have been added to `ActiveEntity` to enable functional, chainable entity operations:

#### `peek( closure )`

Allows peeking into the building process without breaking the chain:

```javascript
user = new User()
    .setFirstName("Luis")
    .peek(function(entity){
        // Inspect or log the entity during building
        writeDump(entity.getMemento());
    })
    .setLastName("Majano")
    .save();
```

#### `when( boolean, successClosure, failureClosure )`

Conditionally execute logic without if statements:

```javascript
user = new User()
    .setEmail(rc.email)
    .when(
        rc.isAdmin,
        function(entity){ entity.setRole("administrator"); },
        function(entity){ entity.setRole("user"); }
    )
    .save();
```

#### `unless( boolean, successClosure, failureClosure )`

The opposite of `when()` - execute when condition is false:

```javascript
user = new User()
    .setActive(true)
    .unless(
        user.hasVerifiedEmail(),
        function(entity){ entity.setActive(false); }
    )
    .save();
```

#### `throwIf( boolean, type, [message], [detail] )`

Throw an exception if a condition is met:

```javascript
user = new User()
    .setAge(rc.age)
    .throwIf(
        user.getAge() < 18,
        "ValidationException",
        "User must be 18 or older"
    )
    .save();
```

#### `throwUnless( boolean, type, [message], [detail] )`

Throw an exception unless a condition is met:

```javascript
user = new User()
    .setEmail(rc.email)
    .throwUnless(
        isValid("email", user.getEmail()),
        "ValidationException",
        "Invalid email address"
    )
    .save();
```

{% hint style="success" %}
These functional helpers enable cleaner, more expressive entity building without breaking the fluent chain or requiring intermediate variables.
{% endhint %}

## Release Date

February 19, 2025
