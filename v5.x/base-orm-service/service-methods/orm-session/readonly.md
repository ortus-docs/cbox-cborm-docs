# readOnly

Run a closure with every entity it loads read-only: they are not dirty-checked and their changes are never saved. It wraps the `bx-orm` `ormReadOnly()` function and returns what the closure returns.

## Returns

* This function returns what the closure returns

## Arguments

| Key      | Type    | Required | Default | Description        |
| -------- | ------- | -------- | ------- | ------------------ |
| callback | closure | Yes      | ---     | The closure to run |

## Examples

```javascript
var report = ormService.readOnly( () => {
    return ormService.list( "Order", { status : "closed" } );
} );
```

For a single call, pass `{ readOnly : true }` as `options` to `get()`, `list()`, `findAllWhere()` or `executeQuery()`.
