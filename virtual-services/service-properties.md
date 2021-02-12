# Service Properties

There are a few properties you can instantiate the virtual service with or set them afterwards that affect operation. Below you can see a nice chart for them:

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `entityName` | string | true |  | The entity name to bind the virtual service with. |
| `queryCacheRegion` | string | false | `#entityName#.defaultVSCache` | The name of the secondary cache region to use when doing queries via this service |
| `useQueryCaching` | boolean | false | false | To enable the caching of queries used by this  service |
| `eventHandling` | boolean | false | true | Announce interception events on _new\(\)_ operations and _save\(\)_ operations: _ORMPostNew, ORMPreSave, ORMPostSave_ |
| `useTransactions` | boolean | false | true | Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities |
| `defaultAsQuery` | boolean | false | true | The bit that determines the default return value for `list(), executeQuery()` as query or array of objects |
| datasource | string | false | System Default | The default datasource to use for all transactions. If not set, we default it to the system datasource or the one declared in the persistent CFC. |

So to create a virtual service you can do this:

```java
component extends="cborm.models.VirtualEntityService"{

  UserService function init(){
      super.init( entityName="User", useQueryCaching=true, eventHandling=false );
      return this;    
  }

}
```

