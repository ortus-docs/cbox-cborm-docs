# executeQuery

Allows the execution of **Custom HQL** queries with binding, pagination, and many options. Underlying mechanism is the `bx-orm` `ormExecuteQuery()` function. The **params** filtering can be using named or positional.

{% hint style="info" %}
Positional parameters can be written as plain `?` or numbered `?1`, `?2` (JPA style), and named parameters as `:name`. Entity and property names in the HQL can be in any case: `bx-orm` resolves them to their declared case.
{% endhint %}

{% hint style="warning" %}
**Changed in cborm 6**: with `unique = true`, more than one matching row raises an `orm.query.nonUnique` error instead of returning the first row. Use [`findIt()`](../finders/findit.md), which limits the query to one row, if you just want the first result.
{% endhint %}

## Returns

This function returns multiple formats:

* array of objects
* array of structs (when you `select new map(...)`)
* a single value or entity (when `unique = true`)
* query (when `asQuery = true`)
* a [cbStreams](https://forgebox.io/view/cbstreams) stream (when `asStream = true`)
* the number of affected records for DML statements (`update`, `insert`, `delete`)

## Arguments

| Key        | Type            | Required | Default            | Description                                                                          |
| ---------- | --------------- | -------- | ------------------ | ------------------------------------------------------------------------------------ |
| query      | string          | Yes      | ---                | The valid HQL to process                                                             |
| params     | array or struct | No       | `{}`               | Positional or named parameters                                                       |
| offset     | numeric         | No       | 0                  | Pagination offset                                                                    |
| max        | numeric         | No       | 0                  | Max records to return                                                                |
| timeout    | numeric         | No       | 0                  | Query timeout                                                                        |
| ignoreCase | boolean         | No       | false              | Case insensitive or case sensitive searches                                          |
| asQuery    | boolean         | No       | `defaultAsQuery`   | Return query or array of objects                                                     |
| unique     | boolean         | No       | false              | Return a unique result                                                               |
| datasource | string          | No       | Service datasource | Use a specific or default datasource                                                 |
| asStream   | boolean         | No       | false              | Returns the result as a [cbStreams](https://forgebox.io/view/cbstreams) stream       |

The service announces the `beforeOrmExecuteQuery` and `afterOrmExecuteQuery` interception points around the query when `eventHandling` is enabled.

## Examples

```javascript
// simple query
ormService.executeQuery( "select distinct a.accountID from Account a" );

// using with list of parameters
ormService.executeQuery(
    "select distinct e.employeeID from Employee e where e.department = ? and e.created > ?",
    [ 'IS', '01/01/2010' ]
);

// same query but with numbered positional parameters and paging
ormService.executeQuery(
    "select distinct e.employeeID from Employee e where e.department = ?1 and e.created > ?2",
    [ 'IS', '01/01/2010' ],
    1,
    30
);

// same query but with named params and paging
ormService.executeQuery(
    "select distinct e.employeeID from Employee e where e.department = :dep and e.created > :created",
    { dep='Accounting', created='01/01/2010' },
    10,
    20
);

// a unique result
var total = ormService.executeQuery(
    query  = "select count(*) from Employee e where e.department = :dep",
    params = { dep : 'IS' },
    unique = true
);

// GET FUNKY!!
```
