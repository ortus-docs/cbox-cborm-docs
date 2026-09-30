# findIt

Finds and returns the first result for the given query or `null` if no entity was found. It runs with `bx-orm`'s `uniqueFirst` option, so it never fails when more than one row matches.

## Returns

* This function returns _any_

## Arguments

| Key        | Type    | Required | Default | Description                               |
| ---------- | ------- | -------- | ------- | ----------------------------------------- |
| query      | string  | No       | ---     | The HQL Query to execute                  |
| params     | any     | No       | {}      | Positional or named params                |
| timeout    | numeric | No       | 0       |                                           |
| ignoreCase | boolean | No       | false   | Ignored, kept for compatibility           |
| datasource | string  | No       |         |                                           |
| options    | struct  | No       | `{}`    | More `bx-orm` `ormExecuteQuery()` options |

## Examples

```javascript
// My First Post
ormService.findIt("from Post as p where p.author='Luis Majano'");
// With positional parameters
ormService.findIt("from Post as p where p.author=?", ["Luis Majano"]);
// With numbered positional parameters
ormService.findIt("from Post as p where p.author=?1", ["Luis Majano"]);
// with named parameters
ormService.findIt("from Post as p where p.author=:author and p.isActive=:active", { author="Luis Majano",active=true} );
```
