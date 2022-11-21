# sessionContains

Checks if the current hibernate session contains the passed in entity.

## Returns

* This function returns _boolean_

## Arguments

| Key    | Type | Required | Default | Description |
| ------ | ---- | -------- | ------- | ----------- |
| entity | any  | Yes      | ---     |             |

## Examples

```javascript
function checkSomething( any User ){
  // check if User is already in session
  if( NOT ormService.sessionContains( arguments.User ) ){
     // Not in hibernate session, so merge it in.
     ormService.merge( arguments.User );
  }
}
```
