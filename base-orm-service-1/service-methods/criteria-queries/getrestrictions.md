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

{% hint style="warning" %}
Adobe ColdFusion will throw an "Invalid CFML construct" for certain CBORM methods that match [reserved operator names](https://helpx.adobe.com/coldfusion/developing-applications/the-cfml-programming-language/elements-of-cfml/reserved-words-in-coldfusion.html), such as `.and()`, `.or()`, and `.eq()`. To avoid these errors and build cross-engine compatible code, use `.$and()`, `.$or()`, and `.isEq()`.
{% endhint %}

