# count

Return the count of instances in the DB for the given entity name. You can also pass an optional where statement that can filter the count. Ex: count\('User','age &gt; 40 AND name="joe"'\). You can even use named or positional parameters with this method: Ex: count\('User','age &gt; ? AND name = ?',\[40,"joe"\]\)

## Returns

* This function returns _numeric_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- |  |
| where | string | No |  |  |
| params | any | No | strucnew\(\) | Named or positional parameters |

## Examples

```javascript
// Get the count of instances for all books
ormService.count("Book");
// Get the count for users with age above 40 and named Bob
ormService.count("User","age > 40 AND name='Bob'");
// Get the count for users with passed in positional parameters
ormService.count("User","age > ? AND name=?",[40,'Bob']);
// Get the count for users with passed in named parameters
ormService.count("Post","title like :title and year = :year",{title="coldbox",year="2007"});
```

