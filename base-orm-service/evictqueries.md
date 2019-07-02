# evictQueries

Evict all queries in the default cache or the cache region that is passed in.

## Returns

* This function returns _void_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| cacheName | string | No | --- |  |
| datasource | string | No | --- | The datasource to use |

## Examples

```javascript
// evict queries that are in the default hibernate cache
ormService.evictQueries();
// evict queries for this service
ormService.evictQueries( ormService.getQueryCacheRegion() );
// evict queries for my artists
ormService.evictQueries( "MyArtits" );
```

