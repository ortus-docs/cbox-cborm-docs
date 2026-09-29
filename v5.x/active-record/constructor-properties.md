---
description: "Constructor Properties for ActiveEntity in CBORM"
icon: gears
---

# Constructor Properties

There are a few properties you can instantiate the **ActiveEntity** with or set them afterwards that affect operation. Below you can see a nice chart for them:

| Property           | Type    | Required | Default                          | Description                                                                                                       |
| ------------------ | ------- | -------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `queryCacheRegion` | string  | false    | `#entityName#.activeEntityCache` | The name of the secondary cache region to use when doing queries via this entity                                  |
| `useQueryCaching`  | boolean | false    | false                            | To enable the caching of queries used by this entity                                                              |
| `eventHandling`    | boolean | false    | true                             | Announce interception events on _new()_ operations and _save()_ operations: _ORMPostNew, ORMPreSave, ORMPostSave_ |
| `useTransactions`  | boolean | false    | true                             | Wraps all operations that save, delete or update ORM entities in a BoxLang `transaction{}` (unless one is running) |
| `defaultAsQuery`   | boolean | false    | false                            | The bit that determines the default return value for `list(), executeQuery()` as query or array of objects        |

The entity name is taken from the `entityName` annotation (or the class name) and the datasource from the `datasource` annotation of the entity, if any.

Here is a nice example of calling the `super.init()` class with some of these constructor properties.

{% code title="User.bx" %}
```javascript
class persistent="true" table="users" extends="cborm.models.ActiveEntity" {

    function init(){

        setCreatedDate( now() );

        super.init( useQueryCaching=true, defaultAsQuery=false );

        return this;
    }

}
```
{% endcode %}
