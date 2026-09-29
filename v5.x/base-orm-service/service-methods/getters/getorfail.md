# getOrFail

Get an entity using a primary key, if the **id** is not found this method throws an `EntityNotFound` Exception

{% hint style="success" %}
No casting is necessary on the Id value type: `bx-orm` converts it to the entity's id type for you.
{% endhint %}

## Returns

* This function returns _any_

## Arguments

| Key        | Type   | Required | Default | Description |
| ---------- | ------ | -------- | ------- | ----------- |
| entityName | string | Yes      | ---     |             |
| id         | any    | Yes      | ---     |             |

## Examples

```javascript
var account = ormService.getOrFail("Account",1);
var account = ormService.getOrFail("Account",4);

// Throws an EntityNotFound exception, it never returns a new entity
var account = ormService.getOrFail("Account",0);
```
