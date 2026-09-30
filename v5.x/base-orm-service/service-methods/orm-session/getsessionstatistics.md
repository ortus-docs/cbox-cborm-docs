# getSessionStatistics

Information about the first-level (session) cache for the current session: `collectionCount`, `collectionKeys`, `entityCount` and `entityKeys`. It delegates to the `bx-orm` `ormGetSessionStatistics()` function.

## Returns

* This function returns _struct_

## Arguments

| Key        | Type   | Required | Default | Description                            |
| ---------- | ------ | -------- | ------- | -------------------------------------- |
| datasource | string | false    | ---     | The default or specific datasource use |

## Examples

```javascript
// Let's get the session statistics
stats = ormService.getSessionStatistics();

// Lets output it
println( "collection count: #stats.collectionCount#" );
println( "entity count: #stats.entityCount#" );
writeDump( stats.entityKeys );
```
