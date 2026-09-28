---
description: "Configuring and tuning Hibernate logging"
icon: file-export
---

# Hibernate Logging

Logs are your best friend when it comes to Hibernate and its plethora of obscure error codes and situations. Hibernate is incredible until it's not. In cborm 6 all ORM logging comes from the `bx-orm` module and the BoxLang logging system.

![](<../.gitbook/assets/image (3).png>)

{% hint style="warning" %}
**Changed in cborm 6**: the `ORMUtilSupport().setupHibernateLogging()` helper and the engine-specific (Lucee and Adobe log4j) setups are gone. Use the settings below.
{% endhint %}

## The ORM Log

bx-orm and Hibernate write their log statements to the `orm.log` file in the BoxLang runtime `logs/` directory (`${boxlang-home}/logs/orm.log`). Raise or lower the level of the `orm` logger in your BoxLang runtime configuration (`boxlang.json`) to see more or less detail. See the [BoxLang logging documentation](https://boxlang.ortusbooks.com/getting-started/configuration/logging) for how loggers are configured.

## Logging SQL

Turn on `logSQL` in your ORM settings to log every SQL statement Hibernate runs:

{% code title="Application.bx" %}
```javascript
this.ormSettings = {
    // ...
    logSQL : true
};
```
{% endcode %}

The statements are written to the same `orm.log` file.

## SQL of a Single Query

You do not need global logging to see what one criteria query runs:

```javascript
var c = userService
    .newCriteria()
    .isEq( "isActive", true )
    .like( "lastName", "M%" );

// The SQL the query will run, without running it
writeDump( c.getSQL() );

// Or record the SQL of the query as you build it
c.startSqlLog()
    .logSQL( "active users" );
writeDump( c.getSqlLog() );
```

See [Help! I'm Not Getting the Result I Expected!](../criteria-queries/help-im-not-getting-the-result-i-expected.md) for more debugging tips.

## ORM Diagnostics

`ormDiagnostics()` returns the ORM state of the current application: status, the last startup error, entities per datasource, warnings and key settings. It never throws, so it is safe to call from a debugging page:

```javascript
writeDump( ormDiagnostics() );
```
