---
description: "Add native SQL conditions to a criteria query with sql()"
---

# SQL Restrictions

When no condition method fits, `sql()` adds a native SQL condition to the criteria. The fragment is sent to the database as written, inside the query the criteria builds, and its values are always bound as parameters.

## Method signature

```javascript
sql( sql, [params] )
sqlRestriction( sql, [params] ) // alias
```

| Argument | Description |
| -------- | ----------- |
| `sql`    | A native SQL boolean condition. Use `?` for each value, `{alias}.column` for a column of the root entity and `{property}` (or `{association.property}`) for a property. |
| `params` | The values for the `?` placeholders, in order: an array (or a comma-separated list). |

The number of values must match the number of `?` placeholders, otherwise an `orm.query.parameter` error is raised. A `?` inside a quoted SQL string literal is not a placeholder.

```javascript
// No parameters
c.sql( "char_length( {alias}.last_name ) = 10" );

// Positional values
c.sql( "{alias}.user_name = ?", [ "joe" ] );
c.sql( "{alias}.user_name = ? and {alias}.first_name like ?", [ "joe", "%joe%" ] );

// Properties instead of column names
c.sql( "upper({firstName}) = ?", [ "LUIS" ] );
c.sql( "lower({role.name}) = ? or {lastName} = ?", [ "admin", "Majano" ] );
```

## Referencing columns and properties

* `{alias}.column` refers to a column of the root entity's table. The column must be mapped by the entity (a property column, an id or an association's foreign key column), otherwise an `orm.property.unknown` error is raised.
* `{property}` refers to a property by name, and `{association.property}` goes through an association, which is joined like any other dotted path.
* Plain, unqualified column names are passed to the database as written. That works when the name is unambiguous in the final SQL, but `{alias}.column` or `{property}` is safer.

## Consider a function path first

Many SQL restrictions only exist to call a database function. A [function path](README.md#function-paths) does that without leaving the criteria API, and keeps property names checked:

```javascript
// Instead of c.sql( "upper({alias}.first_name) = ?", [ "LUIS" ] )
c.isEq( "upper(firstName)", "LUIS" );

// Instead of c.sql( "char_length( {alias}.last_name ) = 10" )
c.isEq( "length(lastName)", 10 );
```

{% hint style="info" %}
**Changes from cborm 5**

* Parameters are plain values. The typed `{ value : ..., type : ... }` structs, the `c.TYPES` map and the type inference rules no longer exist: bx-orm binds each value and the JDBC driver converts it. If a value needs a specific type, convert it in BoxLang before passing it (a number, a date, a boolean).
* Prefer `{alias}.column` or `{property}` over bare column names, since the SQL table aliases Hibernate 7 generates differ from Hibernate 5's.
{% endhint %}
