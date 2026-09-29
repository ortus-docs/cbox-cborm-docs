---
description: "Service Properties - Configuration options for the Base ORM Service in CBORM."
icon: gears
---

# Service Properties

There are a few properties you can instantiate a base service with or set them afterwards that affect operation. Below you can see a nice chart for them:

| Property           | Type    | Required | Default                   | Description                                                                                                                                       |
| ------------------ | ------- | -------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `queryCacheRegion` | string  | false    | `ORMService.defaultCache` | The name of the secondary cache region to use when doing queries via this base service                                                            |
| `useQueryCaching`  | boolean | false    | false                     | To enable the caching of queries used by this base service                                                                                        |
| `eventHandling`    | boolean | false    | **true**                  | Announce interception events on _new()_ operations and _save()_ operations: _ORMPostNew, ORMPreSave, ORMPostSave_                                 |
| `useTransactions`  | boolean | false    | **true**                  | Wraps all operations that save, delete or update ORM entities in a BoxLang `transaction{}` (unless one is already running)                       |
| `defaultAsQuery`   | boolean | false    | **false**                 | The bit that determines the default return value for `list()` and `executeQuery()` as query or array of objects                                   |
| `datasource`       | string  | false    | System Default            | The default datasource to use for all transactions. If not set, we default it to the `ormSettings` datasource or the application datasource. |

So if I was to base off my services on top of the Base Service, I can do this:

```javascript
class extends="cborm.models.BaseORMService" {

  public UserService function init(){
      super.init( useQueryCaching=true, eventHandling=false );
      return this;
  }

}
```

{% hint style="info" %}
The ORM rides the BoxLang `transaction{}` connection, so the writes of a service call commit and roll back together with any native `queryExecute()` in the same transaction. If a transaction is already active, the service joins it instead of starting a new one.
{% endhint %}
