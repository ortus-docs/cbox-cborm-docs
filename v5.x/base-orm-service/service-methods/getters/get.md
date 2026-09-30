# get

Get an entity using a primary key, if the id is not found this method returns null. You can also pass an id = 0 and the service will return to you a new entity.

{% hint style="success" %}
No casting is necessary on the Id value type: `bx-orm` converts it to the entity's id type for you.
{% endhint %}

## Returns

* This function returns _any_

## Arguments

| Key        | Type    | Required | Default | Description                                                                      |
| ---------- | ------- | -------- | ------- | -------------------------------------------------------------------------------- |
| entityName | string  | Yes      | ---     |                                                                                  |
| id         | any     | Yes      | ---     |                                                                                  |
| returnNew  | boolean | false    | true    | If id is 0 or empty and this is true, then a new entity is returned.             |
| options    | struct  | false    | `{}`    | `bx-orm` `entityLoadByPK()` options: `lock`, `timeout`, `skipLocked`, `readOnly` |

## Examples

```javascript
var account = ormService.get("Account",1);
var account = ormService.get("Account",4);

var newAccount = ormService.get("Account",0);

// Read-only, or locked until the transaction ends
var account = ormService.get( "Account", 1, true, { readOnly : true } );
transaction {
    var account = ormService.get( "Account", 1, true, { lock : "write" } );
}
```
