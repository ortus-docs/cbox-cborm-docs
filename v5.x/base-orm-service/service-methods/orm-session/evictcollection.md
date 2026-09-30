# evictCollection

Evict all the collection or association data for a given entity name and collection name from the secondary cache ONLY, not the hibernate session.

Evict an entity name with or without an ID from the secondary cache ONLY, not the hibernate session

## Returns

* This function returns the service (`this`)

## Arguments

| Key          | Type   | Required | Default | Description                                                          |
| ------------ | ------ | -------- | ------- | -------------------------------------------------------------------- |
| entityName   | string | Yes      | ---     | The entity name to evict or use in the eviction process              |
| relationName | string | false    |         | The name of the relation in the entity to evict                      |
| id           | any    | false    |         | The id to use for eviction according to entity name or relation name |

## Examples

```javascript
// Evict all the User entities from the secondary cache
ormService.evictCollection( "User" );
// Evict one User entity from the secondary cache
ormService.evictCollection( entityName = "User", id = 1 );
// Evict the roles collection of all users
ormService.evictCollection( "User", "roles" );
// Evict the roles collection of one user
ormService.evictCollection( "User", "roles", 1 );
```
