---
description: "How to use Active Record Entities in CBORM"
icon: boxes-stacked
---

# Usage

Now that you have created your entities, how do we use them? Well, you will be using them from your handlers or other services by leveraging WireBox's `getInstance()` method or the service `new()` method.  You can use `entityNew()` as well: in cborm 6 the event handler autowires entities created by `entityNew()`, `entityLoadOrNew()` and `entityLoadOrSave()`, and announces `ORMPostNew`, just like entities loaded from the database are autowired when they are loaded (this needs `eventHandling : true` and the cborm `eventHandler`). As best practice, still retrieve everything from WireBox.

Once you have an instance of the entity, then you can use it to satisfy your requirements with the entire gamut of functions available from the [base services](../base-orm-service/service-methods/README.md).

{% hint style="success" %}
**Tip:** You can also check out our [Basic CRUD](../getting-started/basic-crud.md) guide for an initial overview of the usage.
{% endhint %}

```javascript
class {

    function index( event, rc, prc ){
        var user = getInstance( "User" );
        prc.data = user.list( sortOrder="fname" );
        // A Java stream, read from the database as it is consumed
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

## Methods That Default to the Entity

Methods that take an entity default to the active entity itself, so you can call them without arguments: `save()`, `delete()`, `refresh()`, `merge()`, `evict()`, `isDirty()`, `getDirtyPropertyNames()`, `getKeyValue()` and `sessionContains()`.

```javascript
var user = getInstance( "User" ).getOrFail( rc.id );
user.setEmail( rc.email );

if( user.isDirty() ){
    log.info( "Changed: #user.getDirtyPropertyNames().toList()#" );
}
var id = user.getKeyValue();
```

## Locking and Structs

* `lock( mode = "write", options = {} )` locks the entity's row until the current transaction ends (it must run inside a `transaction{}`). `options` takes `timeout` and `skipLocked`.
* `toStruct( options = {} )` returns the entity as a struct via `bx-orm`'s `entityToStruct()`, which honors the entity's `this.memento` includes and excludes. Mementifier's `getMemento()` is still available when mementifier is installed, and it is the default the resource handler uses.

```javascript
transaction {
    var account = getInstance( "Account" ).getOrFail( rc.id ).lock();
    account.setBalance( account.getBalance() - rc.amount ).save();
}

return getInstance( "User" ).getOrFail( rc.id ).toStruct( { includes : "role.name" } );
```
