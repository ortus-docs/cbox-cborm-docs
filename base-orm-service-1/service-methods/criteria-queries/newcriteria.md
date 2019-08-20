# newCriteria

Get a brand new criteria builder object to do criteria queries with \(See [ORM:CriteriaBuilder](../../../criteria-queries/coldbox-criteria-builder/getting-started.md)\)

## Returns

* This function returns _This function returns coldbox.system.orm.hibernate.CriteriaBuilder_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | true | --- | The entity name to bind the criteria query to |
| useQueryCaching | boolean | false | false | Do automatic query caching for queries |
| queryCacheRegion | string | false | criterias.{entityName} | The queryCacheRegion name property for all queries in this criteria object |
| datasource | string | false |  | The datasource to use or default it to the application or entity in use |

## Examples

```javascript
var users = ORMService.newCriteria( entityName="User" )
    .gt( "age", ormService.idCast( 30 ) )
    .isTrue( "isActive" )
    .list( max=30, offset=10, sortOrder="lname" );
```

