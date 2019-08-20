# getRestrictions

Get our hibernate `org.hibernate.criterion.Restrictions` proxy object that will help you produce criterias.

## Returns

* This function returns an instance of `cborm.models.criterion.Restrictions`

## Examples

```javascript
// Get an instance of the restrictions
var restrictions = ormService.getRestrictions();

// Restrictions used with criteria queries
var r = ormService.getRestrictions();
var users = ormService.newCriteria("User")
    .or( r.eq("lastName","majano"), r.gt("createDate", now()) )
    .list();
```

