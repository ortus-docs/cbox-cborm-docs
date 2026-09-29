# Constructor Properties

There are a few properties you can instantiate the **ActiveEntity** with or set them afterwards that affect operation. Below you can see a nice chart for them:

<table data-header-hidden><thead><tr><th width="231">Property</th><th width="150">Type</th><th width="150">Required</th><th width="150">Default</th><th>Description</th></tr></thead><tbody><tr><td>Property</td><td>Type</td><td>Required</td><td>Default</td><td>Description</td></tr><tr><td><code>queryCacheRegion</code></td><td>string</td><td>false</td><td><code>#entityName#.activeEntityCache</code></td><td>The name of the secondary cache region to use when doing queries via this entity</td></tr><tr><td><code>useQueryCaching</code></td><td>boolean</td><td>false</td><td>false</td><td>To enable the caching of queries used by this entity</td></tr><tr><td><code>eventHandling</code></td><td>boolean</td><td>false</td><td>true</td><td>Announce interception events on <em>new()</em> operations and <em>save()</em> operations: <em>ORMPostNew, ORMPreSave, ORMPostSave</em></td></tr><tr><td><code>useTransactions</code></td><td>boolean</td><td>false</td><td>true</td><td>Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities</td></tr><tr><td><code>defaultAsQuery</code></td><td>boolean</td><td>false</td><td>false</td><td>The bit that determines the default return value for <code>list(), executeQuery()</code> as query or array of objects</td></tr></tbody></table>

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
