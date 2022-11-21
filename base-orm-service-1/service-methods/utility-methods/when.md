# when

This method gives you the ability to fluently create chains of executions by evaluating the incoming `target` as a `boolean`. If true it will execute the `success` closure, else the `failure` closure if passed.

## Returns

* The ORM Service so you can do concatenated calls

## Arguments

| Key       | Type      | Required | Default | Description                                   |
| --------- | --------- | -------- | ------- | --------------------------------------------- |
| `target`  | `boolean` | Yes      |         | A boolean evaluator                           |
| `success` | `closure` | Yes      |         | The closure to execute if the target is true  |
| `failure` | `closure` | No       |         | The closure to execute if the target is false |

## Examples

```javascript
baseService
.when( 
    !isNull( rc.createdDate ), 
    ( service ) => service.autoCast( "User", "createdDate", rc.createdDate )
)
.when(
    rc.search.len(),
    ( service ) => service.like( "name", rc.search ),
    ( service ) => service.isTrue( "search" )
)
```
