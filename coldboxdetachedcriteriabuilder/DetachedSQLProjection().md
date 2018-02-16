Besides using it for creating criteria subqueries, Detached Criteria Builder can also be used in conjunction with the new detachedSQLProjection() method to return a projected result based on a subquery. The detachedSQLProjection() method can be called just like any other Criteria Builder projection.

| Transform | Description |
| --- | --- |
| detachedSQLProtection | A single or array of DetachedCriteriaBuilders which will return the projected value |

Examples

```javascript
c = newCriteria();
c.withProjections(
        detachedSQLProjection= 
          c.createSubcriteria( "Car", "Car2" )
           .withProjections( count="Car2.Year" )
           .isLT( "Year", javaCast( "int", 2006 ) )
           .isEQ( "CarID", "{alias}.CarID" ),           
        groupProperty="Make"
).list();
```

> **INFO** If you need to use a property from the root entity in one of your criterias, simply prepend the property name with {alias}. 
> **MORE** otice how a subquery method was not used in this example of the Detached Criteria Builder. 


