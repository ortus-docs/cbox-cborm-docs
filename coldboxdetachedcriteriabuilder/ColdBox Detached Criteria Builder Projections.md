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

