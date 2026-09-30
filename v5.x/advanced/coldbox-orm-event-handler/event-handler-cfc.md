---
description: Extend the cborm event handler to add your own global ORM event logic
---

# Custom Event Handler

![](https://raw.githubusercontent.com/wiki/coldbox-modules/cbox-cborm/ORMEventHandler.jpg)

bx-orm calls one global event handler, the class named in `this.ormSettings.eventHandler`. To add your own global logic while keeping the ColdBox interception points, create a class that extends `cborm.models.EventHandler` and point the setting at it:

{% code title="models/MyEventHandler.bx" %}
```javascript
class extends="cborm.models.EventHandler" {

    public void function preInsert( any entity ){
        // your logic
        arguments.entity.setCreatedBy( getAuthUser() );
        // keep the ColdBox interception points working
        super.preInsert( argumentCollection = arguments );
    }

}
```
{% endcode %}

{% code title="Application.bx" %}
```javascript
this.ormSettings = {
    eventHandling : true,
    eventHandler  : "models.MyEventHandler"
};
```
{% endcode %}

Always call the parent method when you override one, or the matching ColdBox interception point is not announced. Below are the methods you can override:

| Listener Method                          | Description                                                                                                                                  |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `postNew( entity, entityName )`          | Called after a new entity is created by `entityNew()` or a cborm service's `new()` (announced once per `new()`).                              |
| `preLoad( entity )`                      | Called before the data is loaded from the database.                                                                                          |
| `postLoad( entity )`                     | Called after the load operation is complete. The entity is autowired here.                                                                   |
| `preInsert( entity )`                    | Called just before the entity is inserted.                                                                                                   |
| `postInsert( entity )`                   | Called after the insert operation is complete.                                                                                               |
| `preUpdate( entity, oldData )`           | Called just before the entity is updated. `oldData` is a struct of the original state of the entity.                                         |
| `postUpdate( entity )`                   | Called after the update operation is complete.                                                                                               |
| `preDelete( entity )`                    | Called before the entity is deleted.                                                                                                         |
| `postDelete( entity )`                   | Called after the delete operation is complete.                                                                                               |
| `preSave( entity )`                      | Called by the cborm services before a save.                                                                                                  |
| `postSave( entity )`                     | Called by the cborm services after a save.                                                                                                   |
| `postCommit( entity, entityName, action )` | **New in cborm 6**: called once an insert, update or delete is committed.                                                                  |
| `preFlush( entities )`                   | Kept for compatibility: bx-orm does not fire flush events.                                                                                   |
| `postFlush( entities )`                  | Kept for compatibility: bx-orm does not fire flush events.                                                                                   |

