![](ORMEventHandler.jpg)

 The ColdFusion documentation says that in order to create a global event handler that it must implement the *CFIDE.orm.IEventHandler* interface. So all you need to do is create the CFC and make it extend the core class that will provide you with all these capabilities: *cborm.models.EventHandler*

 ```javascript
 component extends="cborm.models.EventHandler"{
}
```

That's it! Just by doing this the CF ORM will call your CFC's event methods in this CFC and satisfy the interface. Of course you can override the methods, but always remember to fire off the parent class methods in order for the ColdBox interceptions to still work. You can then override each event method as needed. Below is a chart of methods you can override:


| Method | Description |
| --- | --- |
| postNew(entity) | This method is called by the ColdBox Base ORM Service Layers after a new entity has been created via its new() method. |
| preLoad(entity) | This method is called before the load operation or before the data is loaded from the database. |
| postLoad(entity) | This method is called after the load operation is complete. |
| preInsert(entity) | This method is called just before the object is inserted. |
| postInsert(entity) | This method is called after the insert operation is complete. |
| preUpdate(Struct oldData,entity) | This method is called just before the object is updated. A struct of old data is passed to this method to know the original state of the entity being updated. |
| postUpdate(entity) | This method is called after the update operation is complete. |
| preDelete(entity) | This method is called before the object is deleted. |
| postDelete(entity) | This method is called after the delete operation is complete. |

The base event handler CFC you inherit from has all the ColdBox interaction capabilities you will ever need. You can find out all its methods by referring to the API and looking at that class or by inspecting the ColdBox Proxy class, which is what is used for enabling ColdBox interactions: *coldbox.system.remote.ColdboxProxy*