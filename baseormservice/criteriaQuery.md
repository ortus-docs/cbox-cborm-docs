#criteriaQuery
Update: Please use our updated [Hibernate Criteria Builder](/coldboxcriteriabuilder/ColdBoxCriteriaBuilder.md) objects instead so you can have much more control, granularity and coolness! 

Do a hibernate criteria based query with projections. You must pass an array of criterion objects by using the Hibernate Restrictions object that can be retrieved from this service using getRestrictions(). The Criteria interface allows to create and execute object-oriented queries. It is powerful alternative to the HQL but has own limitations. Criteria Query is used mostly in case of multi criteria search screens, where HQL is not very effective. 

###Returns

* This function returns *any* which can be an array or query depending on passed arguments


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | any | Yes | --- | The entity name to run the criteria on |
| criteria | array | No | [] | Array of criterion |
| sortOrder  | string | No |  |  |
| offset  | numeric | No | 0 |  |
| max | numeric | No | 0 |  |
| timeout | numeric | No | 0 |  |
| Ignore | boolean | No | false |  |
| asQuery | boolean | No | true |  |

###Examples

```javascript
// Assuming the following code has a dependency on a virtual entity service:
property name="authorService" inject="entityService:Author";

var restrictions = authorService.getRestrictions();
var criteria = [];
ArrayAppend(criteria, Restrictions.eq("firstName","Emily"));
ArrayAppend(criteria, Restrictions.eq("firstName","Paul"));
ArrayAppend(criteria, Restrictions.eq("firstName","Amy"));
example1 = authorService.criteriaQuery([Restrictions.disjunction(criteria)]);

var disjunction = ArrayNew(1);
ArrayAppend(disjunction, Restrictions.eq("firstName","Emily"));
ArrayAppend(disjunction, Restrictions.eq("firstName","Paul"));
ArrayAppend(disjunction, Restrictions.eq("firstName","Amy"));

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.gt("id",JavaCast("int",7)));
ArrayAppend(criteria, Restrictions.disjunction(disjunction));
example2 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.eq("firstName","Michael"));
prc.example1 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.ne("firstName","Michael"));
prc.example2 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.in("firstName",["Ian","Emily","Paul"]));
prc.example3a = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.in("id",JavaCast("java.lang.Integer[]",[2,5,9])));
prc.example3b = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.like("lastName","M%"));
prc.example4 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.ilike("lastName","s%"));
prc.example5 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.isEmpty("books"));
prc.example6 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.isNotEmpty("books"));
prc.example7 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.isNull("bio"));
prc.example8 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.isNotNull("bio"));
prc.example9 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.between("lastName","A","M"));
prc.example10a = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.between("id",JavaCast("int",3),JavaCast("int",7)));
prc.example10b = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.gt("id",JavaCast("int",8)));
prc.example11 = authorService.criteriaQuery(criteria);

var criteria = ArrayNew(1);
ArrayAppend(criteria, Restrictions.ge("id",JavaCast("int",13)));
prc.example12 = authorService.criteriaQuery(criteria);
```