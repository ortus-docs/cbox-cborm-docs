# Projections

When using Detached Criteria Builder for criteria or projection subqueries, you must use a projection. If you think about it from a SQL perspective, this makes sense. After all, we need our subquery to return a specific result (property values, a count, etc.) which will be compared to a property value on the root table (in the case of a criteria subquery), or which will be returned as a valid column value (in the case of a projection subquery).

All of the projections available for Criteria Builder are also available for Detached Criteria Builder.

Examples

```javascript
c = newCriteria();

c.add(
   c.createSubcriteria( ‘Car’, ‘CarSub’ )
     // result of subquery will be CarIDs
    .withProjections( property=’CarID’ )
    .isEq( ‘Make’, ‘Ford’ )
    .propertyIn( ‘CarID’ )
).list();
```

Be careful when using projections and adding criterias to your query. Hibernate does not work well with projections and alias definitions, so if you want to add a criteria you must use `this.yourPropertyName` to tell Hibernate not to use the alias and build a correct SQL.

```javascript
var c = newCriteria();

oReviews = c.withProjections(
    property="status,ID,rating,isComplete" 
  );
c.isTrue( "this.isComplete" );
c.gt( "this.rating", JavaCast( "float", 0 ) );

oReviews = c.resultTransformer( c.ALIAS_TO_ENTITY_MAP ).list();
```
