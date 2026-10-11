# lock

Lock an entity's row in the database until the current transaction ends. It wraps the `bx-orm` `entityLock()` function, so it must run inside a `transaction{}`.

## Returns

* This function returns the entity

## Arguments

| Key     | Type   | Required | Default | Description                                  |
| ------- | ------ | -------- | ------- | -------------------------------------------- |
| entity  | any    | Yes      | ---     | The entity to lock                           |
| mode    | string | No       | `write` | `read`, `write` or `force`                   |
| options | struct | No       | `{}`    | `timeout` (seconds to wait) and `skipLocked` |

## Examples

```javascript
transaction {
    var account = ormService.get( "Account", rc.id );
    ormService.lock( account, "write", { timeout : 5 } );
    account.setBalance( account.getBalance() - rc.amount );
    ormService.save( account );
}
```

You can also lock while loading: `ormService.get( "Account", rc.id, true, { lock : "write" } )`.
