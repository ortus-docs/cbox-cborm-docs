# when

This method gives you the ability to fluently create chains of executions by evaluating the incoming `target` as a `boolean`. If true it will execute the `success` closure, else the `failure` closure if passed. Both closures receive the service as their argument.

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
        rc.keyExists( "clearCache" ),
        ( service ) => service.evictQueries()
    )
    .when(
        rc.flushSession ?: false,
        ( service ) => service.clear(),
        ( service ) => service.evict( prc.user )
    );
```
