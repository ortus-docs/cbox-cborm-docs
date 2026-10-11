# deleteWhere

Deletes entities by using name value pairs as arguments to this function. One mandatory argument is to pass the 'entityName'. The rest of the arguments are used in the where class using AND notation and parameterized. Ex: deleteWhere(entityName="User",age="4",isActive=true);

## Returns

* This function returns _numeric_ (the number of records deleted)

## Arguments

| Key           | Type    | Required | Default            | Description                                              |
| ------------- | ------- | -------- | ------------------ | -------------------------------------------------------- |
| entityName    | string  | Yes      | ---                |                                                          |
| flush         | boolean | No       | false              | Flush the session after the delete                       |
| transactional | boolean | No       | From Property      | Use transactions or not                                  |
| datasource    | string  | No       | Service datasource | Not used: the delete runs on the entity's own datasource |

Any other named argument becomes an `AND` condition of the `where` clause. If you pass no conditions, the method throws a `BaseORMService.NoWhereArgumentsFound` exception instead of deleting everything. This is a DML HQL delete built with a `bx-orm` criteria query, so no cascades or ORM events take place. Each argument name must be a property of the entity (an unknown one raises `orm.property.unknown`), and pending changes in the session are flushed first, so entities you saved earlier in the request are deleted too.

## Examples

```javascript
ormService.deleteWhere(entityName="User", isActive=true, age=10);
ormService.deleteWhere(entityName="Account", id="40");
ormService.deleteWhere(entityName="Book", isReleased=true, author="Luis Majano");
```
