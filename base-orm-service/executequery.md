# executeQuery

Allows the execution of HQL queries using several nice arguments and returns either an array of entities or a query as specified by the asQuery argument. The params filtering can be using named or positional.

## Returns

* This function returns _any_

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| query | string | Yes | --- |  |
| params | any | No | \[runtime expression\] |  |
| offset | numeric | No | 0 |  |
| max | numeric | No | 0 |  |
| timeout | numeric | No | 0 |  |
| asQuery | boolean | No | true |  |
| unique | boolean | No | false | Return a unique result |
| datasource | string | No | --- | Use a specific or default datasource |

## Examples

```javascript
// simple query
ormService.executeQuery( "select distinct a.accountID from Account a" );
// using with list of parameters
ormService.executeQuery( "select distinct e.employeeID from Employee e where e.department = ? and e.created > ?", ['IS','01/01/2010'] );
// same query but with paging
ormService.executeQuery( "select distinct e.employeeID from Employee e where e.department = ? and e.created > ?", ['IS','01/01/2010'],1,30);

// same query but with named params and paging
ormService.executeQuery( "select distinct e.employeeID from Employee e where e.department = :dep and e.created > :created", {dep='Accounting',created='01/01/2010'],10,20);

// GET FUNKY!!
```

