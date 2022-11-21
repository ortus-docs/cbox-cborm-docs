# isSessionDirty

Checks if the session contains dirty objects that are awaiting persistence

## Returns

* This function returns _boolean_

## Arguments

| Key        | Type   | Required | Default | Description                               |
| ---------- | ------ | -------- | ------- | ----------------------------------------- |
| datasource | string | false    | ---     | The default or specific datasource to use |

## Examples

```javascript
// Check if by this point we have a dirty session, then flush it
if( ormService.isSessionDirty() ){
  ORMFlush();
}
```
