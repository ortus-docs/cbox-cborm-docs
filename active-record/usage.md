---
description: "How to use Active Record Entities in CBORM"
icon: boxes-stacked
---

# Usage

Now that you have created your entities, how do we use them? Well, you will be using them from your handlers or other services by leveraging WireBox's `getInstance()` method or the service `new()` method.  You can use `entityNew()` as well, but if you do, the new entity is not autowired by WireBox: in cborm 6 `entityNew()` only announces the `ORMPostNew` interception. Entities loaded from the database are autowired when they are loaded. If you want to leverage DI, as best practice retrieve everything from WireBox.

Once you have an instance of the entity, then you can use it to satisfy your requirements with the entire gamut of functions available from the [base services](../base-orm-service/service-methods/README.md).

{% hint style="success" %}
**Tip:** You can also check out our [Basic CRUD](../getting-started/basic-crud.md) guide for an initial overview of the usage.
{% endhint %}

```javascript
class {

    function index( event, rc, prc ){
        var user = getInstance( "User" );
        prc.data = user.list( sortOrder="fname" );
        // A cbStreams stream
        prc.stream = user.list( sortOrder="fname", asStream=true );
    }

    function count( event, rc, prc ){
        return getInstance( "User" ).countWhere( isActive = true );
    }

    function show( event, rc, prc ){
        return getInstance( "User" )
            .getOrFail( 123 )
            .when( rc.isActive, (user) => user.checkIfActive() )
            .getMemento();
    }

    function save( event, rc, prc ){
        return populateModel( model="User", composeRelationships=true )
            .save()
            .getMemento();
    }

    function delete( event, rc, prc ){
        getInstance( "User" )
            .getOrFail( rc.id ?: -1 )
            .delete();
        return "User Deleted";
    }

}
```
