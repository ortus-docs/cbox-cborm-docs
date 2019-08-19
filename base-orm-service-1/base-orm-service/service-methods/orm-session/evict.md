# evict

Evict entity object\(s\) from the hibernate session or first-level cache.

* An entity object
* An array of entity objects

## Returns

* This function returns _void_

## Arguments

| Key | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| entities | any | Yes | The argument can be one persistence entity or an array of entities to evict |

## Examples

```javascript
ormService.evict( thisEntity );
```

