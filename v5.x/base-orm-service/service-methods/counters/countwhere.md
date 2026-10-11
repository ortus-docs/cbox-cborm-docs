# countWhere

Returns the count by passing name value pairs as arguments to this function. One mandatory argument is to pass the 'entityName'. The rest of the arguments are used in the where class using AND notation and parameterized. Ex: countWhere(entityName="User",age="20");

It runs on a `bx-orm` criteria query: each argument name must be a property of the entity (an unknown one raises `orm.property.unknown`), and the `beforeCriteriaBuilderCount` / `afterCriteriaBuilderCount` interception points are announced.

## Returns

* This function returns _numeric_

## Arguments

| Key        | Type   | Required | Default |
| ---------- | ------ | -------- | ------- |
| entityName | string | Yes      | ---     |

## Examples

```javascript
countWhere( entityName="User", age="20" );
```
