#criteriaCount
Update: Please use our updated [Hibernate Criteria Builder](http://wiki.coldbox.org/wiki/ORM:CriteriaBuilder.cfm) objects instead so you can have much more control, granularity and coolness! 

Get the record count using hibernate projections and criterion for specific queries

###Returns

* This function returns *numeric*


###Arguments

| Key | Type | Required | Default| Description |
| --- | --- | --- | --- | --- |
| entityName | any | Yes | --- | The entity to run count projections on  |
| criteria  | array  | No | [] | The array of hibernate criterion to use for the projections |

###Examples

```javascript


// The following is handler code which has been wired with a virtual entity service

property name="authorService" inject="entityService:Author";

function showAuthors(event){
var rc = event.getCollection();
// Get the hibernate restrictions proxy object
var restrictions = authorService.getRestrictions();
// build criteria
var criteria = [];
// Apply a "greater than" constraint to the named property
ArrayAppend(criteria, restrictions.ge("firstName","M"));

// Get the criteria query first		
prc.example1 = authorService.criteriaQuery(criteria=criteria, offset=(prc.boundaries.STARTROW-1), max=getSetting("PagingMaxRows"), sortOrder="firstName ASC");

// get the total records found via projections
prc.foundcount = authorService.criteriaCount(criteria=criteria);
}
```

