---
description: "Using ColdBox ORM Event Handler to listen to Hibernate ORM events via ColdBox Interceptors"
icon: megaphone
---

# ORM Events

Hibernate can announce events to listener objects so they can tap in to the life-cycle of the entities. You can either listen to the events globally or within the entity itself by just declaring a few listener methods. The cborm module taps in to the global Hibernate events and re-transmits them as[ ColdBox interception points](https://coldbox.ortusbooks.com/digging-deeper/interceptors). This allows you to intercept ORM events via multiple CFC listeners instead of the rigid approach of a single global listener.

![](https://raw.githubusercontent.com/wiki/coldbox-modules/cbox-cborm/ORMEventHandlerBroadcast.jpg)

This is achieved by the `cborm.models.EventHandler` class and by telling the application about it. The event handler also (if configured) will talk to WireBox and auto wire entities with dependencies. Just add your dependency injection properties and off you go with entity injection.

## **Enabling The Event Handler**

You can enable the cborm event handler by opening the `Application.bx` (or `Application.cfc`) and adding two settings:

* `eventHandling`
* `eventHandler`

{% code title="Application.bx" %}
```javascript
this.ormSettings = {
    entityPaths   : [ "models" ],
    dbcreate      : "update",
    logSQL        : true,
    // Enable event handling
    eventHandling : true,
    // The cborm event handler
    eventHandler  : "cborm.models.EventHandler"
};
```
{% endcode %}

That's it, now the cborm event handler will listen to the ORM events, re-broadcast them and you can create [ColdBox Interceptors](https://coldbox.ortusbooks.com/digging-deeper/interceptors) to listen to them.

{% hint style="info" %}
**cborm 6**: `cborm.models.EventHandler` is the one event handler. `cborm.models.BXEventHandler` still works as a deprecated alias.
{% endhint %}

## Listening to Events

Below are the interception points the ORM Event Handler exposes with the data it announces. Just create an interceptor, add the method with the name of the interception point and off you go!

| Interception Point | Data                           | Description                                                                                                  |
| ------------------ | ------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `ORMPostNew`       | `{entity,entityName}`          | Announced once per `new()` on a cborm service or active entity, after the entity is autowired and populated. A plain `entityNew()` announces it too. |
| `ORMPreLoad`       | `{entity}`                     | Called via the `preLoad()` event                                                                             |
| `ORMPostLoad`      | `{entity,entityName}`          | Called via the `postLoad()` event, after the entity is autowired                                             |
| `ORMPreDelete`     | `{entity}`                     | Called via the `preDelete()` event                                                                           |
| `ORMPostDelete`    | `{entity}`                     | Called via the `postDelete()` event                                                                          |
| `ORMPreUpdate`     | `{entity,oldData}`             | Called via the `preUpdate()` event                                                                           |
| `ORMPostUpdate`    | `{entity}`                     | Called via the `postUpdate()` event                                                                          |
| `ORMPreInsert`     | `{entity}`                     | Called via the `preInsert()` event                                                                           |
| `ORMPostInsert`    | `{entity}`                     | Called via the `postInsert()` event                                                                          |
| `ORMPreSave`       | `{entity}`                     | Announced by the cborm services before a `save()`                                                           |
| `ORMPostSave`      | `{entity}`                     | Announced by the cborm services after a `save()`                                                             |
| `ORMPostCommit`    | `{entity,entityName,action}`   | **New in cborm 6**: announced once an insert, update or delete is committed. `action` is `insert`, `update` or `delete` |
| `ORMPreFlush`      | `{entities}`                   | Declared for compatibility: bx-orm does not fire flush events, so it is only announced if you call `preFlush()` yourself |
| `ORMPostFlush`     | `{entities}`                   | Declared for compatibility: bx-orm does not fire flush events, so it is only announced if you call `postFlush()` yourself |

With the exposure of these interception points to your ColdBox application, you can easily create decoupled executable chains of events that respond to ORM events. This really expands the ORM interceptor capabilities to a more decoupled way of listening to ORM events. You can even create different interceptors for different ORM entity classes that respond to the same events, extend the entities with AOP, change entities at runtime, and more; how cool is that.

```javascript
class {

    property name="auditService" inject;

    function ORMPostLoad( event, data, rc, prc ){
        // audit the data.
        var state = data.entity.getMemento();
        auditService.logState( state, data.entity );
    }

    function ORMPostCommit( event, data, rc, prc ){
        // react only once the change is committed
        auditService.logChange( data.entityName, data.action, data.entity );
    }

}
```
