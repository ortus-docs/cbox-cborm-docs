# updateWhere

Update the entities matching a criteria structure with one bulk HQL update, and return how many rows were updated. It runs on a `bx-orm` criteria `updateAll()`.

{% hint style="danger" %}
The update runs in the database only: no entity events, no cascades, no version or `autoTimestamp` changes, and entities already in the session keep their old values until you reload them or clear the session.
{% endhint %}

## Returns

* This function returns _numeric_: the number of updated rows

## Arguments

| Key           | Type    | Required | Default           | Description                                                                                          |
| ------------- | ------- | -------- | ----------------- | ---------------------------------------------------------------------------------------------------- |
| entityName    | string  | Yes      | ---               | The entity to update                                                                                 |
| criteria      | struct  | Yes      | ---               | The property values to match. It cannot be empty: that raises `BaseORMService.NoWhereArgumentsFound` |
| values        | struct  | Yes      | ---               | The property values to set                                                                           |
| transactional | boolean | No       | `useTransactions` | Wrap it in a transaction                                                                             |

## Examples

```javascript
var archived = ormService.updateWhere( "User", { isActive : false }, { isArchived : true } );
```

To update every row, or with conditions other than equality, use a criteria query: `ormService.newCriteria( "User" ).isLt( "lastLogin", cutoff ).updateAll( { isActive : false } )`.
