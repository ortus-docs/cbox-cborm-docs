#Detached Criteria Builder
You can now use the [ORM:DetachedCriteriaBuilder](https://github.com/ColdBox/cbox-cborm/wiki/ORM-Detached-Criteria-Builder) to create programmatically create criteria and projection subqueries.

To create an instance of Detached Criteria Builder, simply call the *createSubcriteria()* method on your existing criteria.

The arguments for *createSubcriteria()* are:

| Argument | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | true | --- | The name of the entity to bind this detached criteria builder with. |
| alias | string | true | --- | The alias to use |

Once the Detached Criteria Builder is defined, you can add projections, criterias, and associations, just like you would with Criteria Builder.

Examples

```javascript
// criteria builder
c = newCriteria();
// detached criteria builder
c.createSubcriteria( 'Car', 'CarSub' );   
```

See the documentation for [ORM:DetachedCriteriaBuilder](https://github.com/ColdBox/cbox-cborm/wiki/ORM-Detached-Criteria-Builder) for more information.