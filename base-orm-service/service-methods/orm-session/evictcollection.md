# evictCollection

Evict all the collection or association data for a given entity name and collection name from the secondary cache ONLY, not the hibernate session.&#x20;

Evict an entity name with or without an ID from the secondary cache ONLY, not the hibernate session

## Returns

* This function returns _void_

## Arguments

| Key          | Type   | Required | Default | Description                                                          |
| ------------ | ------ | -------- | ------- | -------------------------------------------------------------------- |
| entityName   | string | Yes      | ---     | The entity name to evict or use in the eviction process              |
| relationName | string | false    |         | The name of the relation in the entity to evict                      |
| id           | any    | false    |         | The id to use for eviction according to entity name or relation name |

## Examples

```javascript
```
