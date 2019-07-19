# Service Properties

There are a few properties you can instantiate a base service with or set them afterwards that affect operation. Below you can see a nice chart for them:

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `queryCacheRegion` | string | false | `ORMService.defaultCache` | The name of the secondary cache region to use when doing queries via this base service |
| `useQueryCaching` | boolean | false | false | To enable the caching of queries used by this base service |
| `eventHandling` | boolean | false | true | Announce interception events on _new\(\)_ operations and _save\(\)_ operations: _ORMPostNew, ORMPreSave, ORMPostSave_ |
| `useTransactions` | boolean | false | true | Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities |
| `defaultAsQuery` | boolean | false | true | The bit that determines the default return value for list\(\), createCriteriaQuery\(\) and executeQuery\(\) as query or array of objects |

So if I was to base off my services on top of this gem, I can do this:

```javascript
component extends="cborm.models.BaseORMService"{

  public UserService function init(){
      super.init( useQueryCaching=true, eventHandling=false );
      return this;    
  }

}
```

