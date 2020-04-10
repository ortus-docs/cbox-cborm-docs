# findit

Finds and returns the first result for the given query or null if no entity was found. You can either use the query and params combination or send in an example entity to find.

## Returns

* This function returns _any_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| query | string | No | --- | The HQL Query to execute |
| params | any | No | {} | Positional or named params |
| timeout | numeric | No | 0 |  |
| ignoreCase | boolean | No | false |  |
| datasource | string | No |  |  |

## Examples

```javascript
// My First Post
ormService.findIt("from Post as p where p.author='Luis Majano'");
// With positional parameters
ormService.findIt("from Post as p where p.author=?", ["Luis Majano"]);
// with a named parameter (since 0.5)
ormService.findIt("from Post as p where p.author=:author and p.isActive=:active", { author="Luis Majano",active=true} );
```

