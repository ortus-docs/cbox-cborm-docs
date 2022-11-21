# findOrFail

Finds and returns the first result for the given query or throws an exception if not found, this method delegates to the `findIt()` method

## Returns

* This function returns _any_
* This function could throw an **EntityNotFound** exception

## Arguments

| Key        | Type    | Required | Default | Description                |
| ---------- | ------- | -------- | ------- | -------------------------- |
| query      | string  | No       | ---     | The HQL Query to execute   |
| params     | any     | No       | {}      | Positional or named params |
| timeout    | numeric | No       | 0       |                            |
| ignoreCase | boolean | No       | false   |                            |
| datasource | string  | No       |         |                            |

## Examples

```javascript
// My First Post
ormService.findOrFail("from Post as p where p.author='Luis Majano'");
// With positional parameters
ormService.findOrFail("from Post as p where p.author=?", ["Luis Majano"]);
// with a named parameter (since 0.5)
ormService.findOrFail("from Post as p where p.author=:author and p.isActive=:active", { author="Luis Majano",active=true} );
```
