# deleteWhere

Deletes entities by using name value pairs as arguments to this function. One mandatory argument is to pass the 'entityName'. The rest of the arguments are used in the where class using AND notation and parameterized. Ex: deleteWhere(entityName="User",age="4",isActive=true);

## Returns

* This function returns _numeric_ (the number of records deleted)

## Arguments

| Key           | Type    | Required | Default       | Description          |
| ------------- | ------- | -------- | ------------- | -------------------- |
| entityName    | string  | Yes      | ---           |                      |
| flush         | boolean | No       | false         | Not used: the DML delete runs right away |
| transactional | boolean | No       | From Property | Use transactions or not |
| datasource    | string  | No       | Service datasource | The datasource to use |

Any other named argument becomes an `AND` condition of the `where` clause. If you pass no conditions, the method throws a `BaseORMService.NoWhereArgumentsFound` exception instead of deleting everything. This is a DML HQL delete, so no cascades or ORM events take place.

## Examples

```javascript
ormService.deleteWhere(entityName="User", isActive=true, age=10);
ormService.deleteWhere(entityName="Account", id="40");
ormService.deleteWhere(entityName="Book", isReleased=true, author="Luis Majano");
```
