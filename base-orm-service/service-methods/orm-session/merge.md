# merge

Merge an entity or array of entities back into the session

## Returns

* Same entity if one passed, array if an array of entities passed.

## Arguments

| Key    | Type | Required | Default | Description |
| ------ | ---- | -------- | ------- | ----------- |
| entity | any  | Yes      | ---     | A single entity or an array of entities to merge |

## Examples

```javascript
// merge a single entity back
ormService.merge( userEntity );
// merge an array of entities
collection = [entity1,entity2,entity3];
ormService.merge( collection );
```
