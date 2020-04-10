# Constructor Properties

There are a few properties you can instantiate the **ActiveEntity** with or set them afterwards that affect operation. Below you can see a nice chart for them:

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `queryCacheRegion` | string | false | `#entityName#.activeEntityCache` | The name of the secondary cache region to use when doing queries via this entity |
| `useQueryCaching` | boolean | false | false | To enable the caching of queries used by this entity |
| `eventHandling` | boolean | false | true | Announce interception events on _new\(\)_ operations and _save\(\)_ operations: _ORMPostNew, ORMPreSave, ORMPostSave_ |
| `useTransactions` | boolean | false | true | Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities |
| `defaultAsQuery` | boolean | false | true | The bit that determines the default return value for `list(), executeQuery()` as query or array of objects |

Here is a nice example of calling the `super.init()` class with some of these constructor properties.

{% code title="User.cfc" %}
```javascript
component persistent="true" table="users" extends="cborm.models.ActiveEntity"{

    function init(){

        setCreatedDate( now() );

        super.init( useQueryCaching=true, defaultAsQuery=false );

        return this;
    }

}
```
{% endcode %}

