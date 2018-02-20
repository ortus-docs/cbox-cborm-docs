#newCriteria
Get a brand new criteria builder object to do criteria queries with (See [ORM:CriteriaBuilder](/coldboxcriteriabuilder/ColdBox-Criteria-Builder-Getting-Started.md))

###Returns

* This function returns *This function returns coldbox.system.orm.hibernate.CriteriaBuilder*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | true | --- | The entity name to bind the criteria query to |
| useQueryCaching | boolean | false | false | Do automatic query caching for queries |
| queryCacheRegion | string | false | criterias.{entityName} | The queryCacheRegion name property for all queries in this criteria object |

###Examples

```javascript
var users = ORMService.newCriteria(entityName="User",useQueryCaching=true)
	.gt("age", javaCast("int", 30) )
	.isTrue("isActive")
	.list(max=30,offset=10,sortOrder="lname");
```


