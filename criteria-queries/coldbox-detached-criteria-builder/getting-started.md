# Getting Started

Whether you want to create a criteria subquery, or a projection subquery, you'll first need to get a new instance of the Detached Criteria Builder class. Since all of the subqueries we're creating are being added to our main criteria query either as a criteria or a projection, we can get the Detached Criteria Builder like so:

```javascript
// Get a reference to a criteria builder from an ORM base or virtual service
c = ormservice.newCriteria();

// detached criteria builder
c.createSubcriteria( entityName='Car', alias='CarSub' );
```

The arguments for the _createSubcriteria\(\)_ method are:

| Argument | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | true | --- | The name of the entity to bind this detached criteria builder with. |
| alias | string | true | --- | The alias to use for the entity |

Once the Detached Criteria Builder is defined, you can add projections, criterias, and associations, just like you would with a normal Criteria Builder.

Examples

```javascript
c.newCriteria( entityName=‘Car’ );

// all-in-one criteria subquery
c.add(
   c.createSubcriteria( ‘Car’, ‘CarSub’ )
    .withProjections( property=’CarID’ )
    .isEq( ‘Make’, ‘Ford’ )
    .propertyIn( ‘CarID’ )
).list();

// detached criteria builder separately, then add as criteria
var dc = c.createSubcriteria( ‘Car’, ‘CarSub’ )
          .withProjections( property=’CarID’ )
          .isEq( ‘Make’, ‘Ford’ );
// add criteria subquery
c.add( dc.propertyIn( ‘CarID’ ) );
```

