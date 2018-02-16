Get our hibernate org.hibernate.criterion.Restrictions proxy object

###Returns

* This function returns *coldbox.system.orm.hibernate.criterion.Restrictions*

###Examples

```javascript
// Restrictions used with criteria queries
var r = ormService.getRestrictions();
var users = ormService.newCriteria("User")
	.or( r.eq("lastName","majano"), r.gt("createDate", now()) )
	.list();
```