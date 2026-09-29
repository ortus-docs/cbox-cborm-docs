# executeQuery

Allows the execution of **Custom HQL** queries with binding, pagination, and many options. Underlying mechanism is `ORMExecuteQuery`. The **params** filtering can be using named or positional.

## Returns

This function returns multiple formats:

* array of objects
* array of structs
* query
* cbStream

## Arguments

| Key        | Type            | Required | Default | Description                                                                               |
| ---------- | --------------- | -------- | ------- | ----------------------------------------------------------------------------------------- |
| query      | string          | Yes      | ---     | The valid HQL to process                                                                  |
| params     | array or struct | No       |         | Positional or named parameters                                                            |
| offset     | numeric         | No       | 0       | Pagination offset                                                                         |
| max        | numeric         | No       | 0       | Max records to return                                                                     |
| timeout    | numeric         | No       | 0       | Query timeout                                                                             |
| asQuery    | boolean         | No       | false   | Return query or array of objects                                                          |
| unique     | boolean         | No       | false   | Return a unique result                                                                    |
| datasource | string          | No       | ---     | Use a specific or default datasource                                                      |
| asStream   | boolean         | No       | false   | Returns the result as a Java Stream using [cbStreams](https://forgebox.io/view/cbstreams) |

## Examples

```javascript
// simple query
ormService.executeQuery( "select distinct a.accountID from Account a" );

// using with list of parameters
ormService.executeQuery( 
    "select distinct e.employeeID from Employee e where e.department = ? and e.created > ?", 
    [ 'IS', '01/01/2010' ] 
);

// same query but with paging
ormService.executeQuery( 
    "select distinct e.employeeID from Employee e where e.department = ? and e.created > ?", 
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

// GET FUNKY!!
```
