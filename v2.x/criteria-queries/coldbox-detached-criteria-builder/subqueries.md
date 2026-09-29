# Subqueries

If you are using Detached Criteria Builder for a criteria subquery, you will also need to use one of the methods from the ColdBox subqueries class. This is what will ultimately bind the subquery result to the root entity.

The most important thing to remember is that this subquery is what needs to be added as a criterion to your criteria query. So whether you are doing a simple subquery or building up a complex detached criteria, the result of one of the methods below should be what is added as a criterion to the criteria query (see example below).

Examples

```javascript
// wrong way...will fail because the subquery method “propertyIn()” is not what is added
c.add(
   c.createSubcriteria( ‘Car’, ‘CarSub’ )
    .withProjections( property=’CarID’ )
    .propertyIn( ‘CarID’ )
    .isEq( ‘Make’, ‘Ford’ )
).list();

// right way...since propertyIn() is last in the chain, it’s value will be what is ultimately added as a criteria
c.add(
   c.createSubcriteria( ‘Car’, ‘CarSub’ )
    .withProjections( property=’CarID’ )
    .isEq( ‘Make’, ‘Ford’ )
    .propertyIn( ‘CarID’ )
).list();

// right way--split up
dc = c.createSubcriteria( ‘Car’, ‘CarSub’ )
  .withProjections( property=’CarID’ )
  .isEq( ‘Make’, ‘Ford’ );
c.add( dc.propertyIn( ‘CarID’ ) ).list();
```

| Method                                   | Description                                                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| detachedSQLProjection                    | A single or array of DetachedCriteriaBuilders which will return the projected value                           |
| subEq(value)                             | Where the result of the subquery is eq to a specific value                                                    |
| subEqAll(value)                          | Where all values in the subquery result are equal to a specific value                                         |
| subGe(value)                             | Where values in the subquery result are greater than or equal to a specific value                             |
| subGeAll(value)                          | Where all values in the subquery result are equal to a specific value                                         |
| subGeSome(value)                         | Where some values in the subquery result are greater than a specific value                                    |
| subGt(value)                             | Where values in the subquery result are greater than a specific value                                         |
| subGtAll(required string value)          | Where all values in the subquery result are greater than a specific value                                     |
| subGtSome(required string value)         | Where some values in the subquery result are greater than a specific value                                    |
| subIn(required string value)             | Where values in the subquery result are contained within the specified list of values                         |
| subLe(required string value)             | Where values in the subquery result are less than or equal to a specific value                                |
| subLeAll(required string value)          | Where all values in the subquery result are less than or equal to a specific value                            |
| subLeSome(required string value)         | Where some values in the subquery result are less than or equal to a specific value                           |
| subLt(required string value)             | Where values in the subquery result are less than a specific value                                            |
| subLtAll(required string value)          | Where all values in the subquery result are less than a specific value                                        |
| subLtSome(required string value)         | Where some values in the subquery result are less than a specific value                                       |
| subNe(required string value)             | Where values in the subquery result are not equal to a specific value                                         |
| subNotIn(required string value)          | Where values in the subquery result are not contained within the specified list of values                     |
| exists                                   | Where the subquery returns some result                                                                        |
| notExists                                | Where the subquery does not return a result                                                                   |
| propertyEq(required string property)     | Where values in the subquery result are equal to the the specified property value                             |
| propertyEqAll(required string property)  | Where all values in the subquery result are equal to the the specified property value                         |
| propertyGe(required string property)     | Where values in the subquery result are greater than or equal to the the specified property value             |
| propertyGeAll(required string property)  | Where all values in the subquery result are greater than or equal to the the specified property value         |
| propertyGeSome(required string property) | Where some values in the subquery result are greater than or equal to the the specified property value        |
| propertyGt(required string property)     | Where values in the subquery result are greater than the the specified property value                         |
| propertyGtAll(required string property)  | Where all values in the subquery result are greater than the the specified property value                     |
| propertyGtSome(required string property) | Where some values in the subquery result are greater than the the specified property value                    |
| propertyIn(required string property)     | Where values in the subquery result are contained in within the list of values for the specified property     |
| propertyLe(required string property)     | Where values in the subquery result are less than or equal to the the specified property value                |
| propertyLeAll(required string property)  | Where all values in the subquery result are less than or equal to the the specified property value            |
| propertyLeSome(required string property) | Where some values in the subquery result are less than or equal to the the specified property value           |
| propertyLt(required string property)     | Where values in the subquery result are less than the the specified property value                            |
| propertyLtAll(required string property)  | Where all values in the subquery result are less than the the specified property value                        |
| propertyLtSome(required string property) | Where some values in the subquery result are less than the the specified property value                       |
| propertyNe(required string property)     | Where values in the subquery result are not equal to the the specified property value                         |
| propertyNotIn(required string property)  | Where values in the subquery result are not contained in within the list of values for the specified property |
