#deleteWhere
Deletes entities by using name value pairs as arguments to this function. One mandatory argument is to pass the 'entityName'. The rest of the arguments are used in the where class using AND notation and parameterized. Ex: deleteWhere(entityName="User",age="4",isActive=true);

###Returns
* This function returns *numeric*

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- |  |
| transactional | boolean | No | From Property | Use transactions not |

###Examples

```javascript
ormService.deleteWhere(entityName="User", isActive=true, age=10);
ormService.deleteWhere(entityName="Account", id="40");
ormService.deleteWhere(entityName="Book", isReleased=true, author="Luis Majano");
```

