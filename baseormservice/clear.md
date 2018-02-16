Clear the session removes all the entities that are loaded or created in the session. This clears the first level cache and removes the objects that are not yet saved to the database.

#### Returns

* This function returns *void*

#### Examples

```javascript
ormService.clear();
```