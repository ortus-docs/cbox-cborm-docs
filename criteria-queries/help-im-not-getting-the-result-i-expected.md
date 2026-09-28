---
description: "Troubleshooting unexpected results in Criteria Queries with CBORM"
icon: life-ring
---

# Help! I'm Not Getting the Result I expected

Since you're not writing SQL, it can sometimes be hard to see why the results of a criteria query don't match what you expect. The fix is almost always to look at the query that actually runs.

## 1. Look at the query

The criteria builder can show you its HQL and SQL without running anything:

```javascript
var c = userService
    .newCriteria()
    .isTrue( "isActive" )
    .or( ( c ) => c.isEq( "role.slug", "admin" ).isGt( "age", 30 ) );

// The SQL, with the values in place, ready to paste into your SQL tool
writeOutput( c.getSQL( returnExecutableSql = true ) );

// The calls you made, the HQL, the parameters and the SQL
writeDump( c );
```

Run the SQL in your database tool (MySQL Workbench, SQL Server Management Studio, DBeaver, ...), find where it differs from what you meant, and adjust the criteria until it's right. To follow how the query changes while you build it, use `peekSQL()`, or `logSQL()` and `getSqlLog()`. See [SQL Log & Debugging](criteria-builder/sql-log.md).

## 2. Log the SQL that runs

To see every statement the ORM runs, including lazy loads and flushes, turn on SQL logging in the ORM settings of your `Application.bx`:

```javascript
this.ormSettings = {
    // ...
    logSQL : true
};
```

## 3. Common surprises

* **Rows go missing when you filter on an association.** A condition on a dotted path uses an inner join, so rows without the association are left out (inside `or()` and `not()` a left join is used instead). To keep them, join with `leftJoin( "role", "r" )` and allow the missing case in your conditions, for example `or( ( c ) => c.isEq( "r.slug", "admin" ).isNull( "r.id" ) )`. See [Associations](criteria-builder/associations.md).
* **`get()` raises `orm.query.nonUnique`.** More than one row matches. Tighten the conditions, or call `get( uniqueFirst = true )` or `first()`.
* **`getOrFail()` raises `orm.notFound`.** No row matches: check the SQL and the values.
* **A misspelled property raises `orm.property.unknown`.** The error suggests the closest property name. Property names are not case-sensitive, so casing is never the problem.
* **Results are in an unexpected order.** Without `order()`, entity results follow the entity's `defaultSort` when it declares one, otherwise the database's order.
* **Changes made by `updateAll()` or `deleteAll()` don't show on loaded entities.** Bulk statements bypass the session: reload the entities or clear the session.
* **`getSQL()` raises an error about a statement inspector.** Your ORM settings configure their own `hibernate.session_factory.statement_inspector`, which `getSQL()` can't share.

{% hint style="success" %}
**Tip**: You don't have to use the ORM for everything. If a criteria query fights you for more than a few minutes, write it in HQL with `executeQuery()` or in SQL with `queryExecute()`.
{% endhint %}
