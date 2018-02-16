The ColdBox restrictions class allows you to create criterions upon certain properties, associations and even SQL for your ORM entities. This is the meat and potatoes of criteria queries, where you build a set of criterion to match against. The ColdBox criteria class offers almost all of the criterion methods found in the native hibernate Restrictions class (http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html) but if you need to add very explicit criterion directly you have access to the ColdBox Restrictions class which proxies calls to the native Hibernate class. You do this by either retrieving it from the Base/Virtual ORM services (getRestrictions()), create it manually, or the Criteria object itself has a public property called restrictions which you can use rather easily. We prefer the latter approach. Now, plese understand that the ColdBox Criteria Builder masks all this complexity and in very rare cases will you be going to our restrictions class directly. Most of the time you will just be happily concatenating methods on the Criteria Builder.

```javascript

// From base ORM service
var restrictions = getRestrictions()

// Manually Created
var restrictions = new coldbox.system.orm.hibernate.criterion.Restrictions();

// From Criteria Builder
newCriteria().restrictions
```

Ok, now that the formalities have been explained let's build some criterias.