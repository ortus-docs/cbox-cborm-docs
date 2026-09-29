# Service Properties

There are a few properties you can instantiate the virtual service with or set them afterwards that affect operation. Below you can see a nice chart for them:

<table data-header-hidden><thead><tr><th width="226">Property</th><th width="150">Type</th><th width="150">Required</th><th width="150">Default</th><th>Description</th></tr></thead><tbody><tr><td>Property</td><td>Type</td><td>Required</td><td>Default</td><td>Description</td></tr><tr><td><code>entityName</code></td><td>string</td><td>true</td><td></td><td>The entity name to bind the virtual service with.</td></tr><tr><td><code>queryCacheRegion</code></td><td>string</td><td>false</td><td><code>#entityName#.defaultVSCache</code></td><td>The name of the secondary cache region to use when doing queries via this service</td></tr><tr><td><code>useQueryCaching</code></td><td>boolean</td><td>false</td><td>false</td><td>To enable the caching of queries used by this  service</td></tr><tr><td><code>eventHandling</code></td><td>boolean</td><td>false</td><td>true</td><td>Announce interception events on <em>new()</em> operations and <em>save()</em> operations: <em>ORMPostNew, ORMPreSave, ORMPostSave</em></td></tr><tr><td><code>useTransactions</code></td><td>boolean</td><td>false</td><td>true</td><td>Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities</td></tr><tr><td><code>defaultAsQuery</code></td><td>boolean</td><td>false</td><td>false</td><td>The bit that determines the default return value for <code>list(), executeQuery()</code> as query or array of objects</td></tr><tr><td>datasource</td><td>string</td><td>false</td><td>System Default</td><td>The default datasource to use for all transactions. If not set, we default it to the system datasource or the one declared in the persistent CFC.</td></tr></tbody></table>

To create a virtual service you can do this:

```java
component extends="cborm.models.VirtualEntityService"{

  UserService function init(){
      super.init( entityName="User", useQueryCaching=true, eventHandling=false );
      return this;    
  }

}
```
