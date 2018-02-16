![](ORMEventHandlerBroadcast.jpg)

We have also expanded the hibernate ORM events to bridge the gap to the ColdBox interceptors. So now all the hibernate interceptors will relay their events to ColdBox via interceptors. This means that each hibernate event, like *preLoad()* for example, will announce its ColdBox counterpart: *ORMPreLoad()*. Below are the new interception points the ORM Event Handler exposes with the appropriate interception data it announces: 

| Interception Point | Intercept Structure | Description |
| --- | --- | --- |
| ORMPostNew | {entity} | Called via the postNew() event |
| ORMPreLoad | {entity} | Called via the preLoad() event |
| ORMPostLoad | {entity} | Called via the postLoad() event |
| ORMPostDelete | {entity} | Called via the postDelete() event |
| ORMPreDelete | {entity} | Called via the preDelete() event |
| ORMPreUpdate |  {entity,oldData} | Called via the preUpdate() event |
| ORMPostUpdate | {entity} | Called via the postUpdate() event |
| ORMPreInsert | {entity} | Called via the preInsert() event |
| ORMPostInsert | {entity} | Called via the postInsert() event |


With the exposure of these interception points to your ColdBox application, you can easily create decoupled executable chains of events that respond to ORM events. This really expands the ORM interceptor capabilities to a more decoupled way of listening to ORM events. You can even create different interceptors for different ORM entity classes that respond to the same events, extend the entities with AOP, change entities at runtime, and more; how cool is that.