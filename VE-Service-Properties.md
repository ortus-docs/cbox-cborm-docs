Here are the properties you can set or construct virtual entity services with:

| Property | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | true | --- | The entity name you want to bound this virtual service to |
| datasource | string | false | *entity datasource* | The datasource this entity service will be binded too. If not passed it defaults to whatever the entity is binded to refers. |
| queryCacheRegion | string | false | *ORMService.defaultCache* | The name of the secondary cache region to use when doing queries via this base service |
| useQueryCaching | boolean | false | false | To enable the caching of queries used by this base service |
| eventHandling | boolean | false | true | Announce interception events on new() operations and save() operations: ORMPostNew, ORMPreSave, ORMPostSave |
| useTransactions | boolean | false | true | Enables ColdFusion safe transactions around all operations that either save, delete or update ORM entities |
| defaultAsQuery | boolean | false | true | The bit that determines the default return value for list(), createCriteriaQuery() and executeQuery() as query or array of objects |

```javascript
component extends="cborm.models.VirtualEntityService" singleton{

	/**
	* Constructor
	*/
	MyService function init(){
		// super init it
		super.init(entityName="MyEntity", useQueryCaching=true, defaultAsQuery=false);

		return this;
	}
}
```
