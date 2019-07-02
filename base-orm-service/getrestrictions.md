# getRestrictions

Get our hibernate org.hibernate.criterion.Restrictions proxy object

## Returns

* This function returns _coldbox.system.orm.hibernate.criterion.Restrictions_

## Examples

```javascript
// Restrictions used with criteria queries
var r = ormService.getRestrictions();
var users = ormService.newCriteria("User")
    .or( r.eq("lastName","majano"), r.gt("createDate", now()) )
    .list();
```

