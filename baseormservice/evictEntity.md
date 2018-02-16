Evict entity objects from session. The argument can be one persistence entity or an array of entities

###Returns

* This function returns *void*

###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entity | any | Yes | --- |  |

###Examples

```javascript
// evict one entity
ORMService.evictEntity( entity );

// evict an array of entities
entities = [ user1, user2 ];
ORMService.evictEntity( entities );
```

