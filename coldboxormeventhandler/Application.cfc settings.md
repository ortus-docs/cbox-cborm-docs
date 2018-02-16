```javascript
this.ormSettings = {
    cfclocation="model",
    dbcreate = "update",
    dialect = "MySQLwithInnoDB",
    logSQL = true,
    // Enable event handling
    eventhandling = true,
    // Set the event handler to use, which will be inside our application.
    eventhandler = "model.ORMEventHandler"
};
```

In this basic ORM configuration structure I have two lines that deal with ORM events:

```javascript
// Enable event handling
eventhandling = true,
// Set the event handler to use, which will be inside our application.
eventhandler = "model.ORMEventHandler"
            
```

This enables the internal Hibernate interceptors and then you map a global CFC that will handle them. Mine will be in my local application's model folder named ORMEventHandler. It has to be inside of my application's folder structure in order to be able to talk to the ColdBox application. This is extremely important, as this creates the bridge between the ORM and ColdBox.

> **Imporant** The ORM Event Handler CFC MUST exist within the application in order for the bridge to work. 