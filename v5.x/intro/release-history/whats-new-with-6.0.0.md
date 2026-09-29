---
description: What's new in cborm 6.0.0, the pure BoxLang release built on bx-orm 2 and Hibernate 7
---

# What's New With 6.0.0

cborm 6.0.0 is a major release. cborm is now a **pure BoxLang module** built on top of the [bx-orm](https://forgebox.io/view/bx-orm) 2 module, which runs Hibernate ORM 7.4. Your services, virtual services, active entities, dynamic finders and criteria queries keep working, and they gain everything bx-orm 2 brings: clearer errors, a faster boot, case-insensitive names and a modern criteria engine.

{% hint style="warning" %}
Adobe ColdFusion and Lucee are no longer supported. The cborm 5.x series keeps supporting them (with `bx-orm` 1 on BoxLang). CFML applications can run cborm 6 on BoxLang through the `bx-compat-cfml` module. Read [Upgrading to 6](upgrading-to-6.md) before you upgrade.
{% endhint %}

## Pure BoxLang

* Every module source is a BoxLang class (`.bx`), including `ModuleConfig.bx`.
* The module is tested on BoxLang and on BoxLang with `bx-compat-cfml`, so CFML (`.cfc`) applications keep working.
* Requirements: BoxLang 1.17.5+, `bx-orm` 2, ColdBox 8+.

## Hibernate 7 Through bx-orm 2

Hibernate 7 removed the legacy Criteria API that cborm 5 wrapped (`org.hibernate.Criteria`, `Restrictions`, `DetachedCriteria`, `Projections`, `ClassMetadata`, `EntityMode`). cborm 6 no longer talks to Hibernate directly: `BaseORMService`, `VirtualEntityService` and `ActiveEntity` keep their API and delegate to the bx-orm functions.

## Criteria Queries on `entityCriteria()`

`newCriteria()` now returns the bx-orm `entityCriteria()` builder. It keeps cborm's method names, so most criteria code runs unchanged, and adds:

* `c.restrictions` (and `getRestrictions()`) condition objects you can pass to `add()`, `or()`, `and()` and `not()`.
* Subqueries with `c.subquery( "Entity", "alias" )`, including the quantified forms (`subGeAll`, `subLtSome`, `propertyEqAll`, `propertyLtSome`, ...).
* Projections with `withProjections()` and struct results with `asStruct()`.
* The SQL log: `startSqlLog()`, `logSQL()`, `getSqlLog()`, `stopSqlLog()`, `canLogSql()`, plus `getSQL()` and `peekSQL()` to see the SQL without running it.
* Property names checked while you build the query, with a "Did you mean" suggestion for misspellings.

See [Criteria Builder](../../criteria-queries/criteria-builder/README.md).

## Case-Insensitive Names

HQL entity and property names resolve in any case, as the rest of BoxLang does: `executeQuery( "from user where firstname = ?", [ "Luis" ] )` finds the `User` entity's `firstName` property. Criteria queries and dynamic finders (including their `sortBy` option) accept any case too.

## Events

* New `ORMPostCommit` interception point, announced once an insert, update or delete is committed. Data: `entity`, `entityName`, `action`.
* `ORMPostNew` is announced once per `new()`, after the entity is autowired and populated. A plain `entityNew()` announces it too.
* One event handler: `cborm.models.EventHandler`. `BXEventHandler` remains as a deprecated alias.
* The bx-orm criteria events (`onCriteriaBuilderAddition`, `beforeCriteriaBuilderList`, ...) are relayed to your ColdBox interceptors.

## Transactions

Service transactions and the `HibernateTransaction` aspect ride BoxLang `transaction{}` blocks, so ORM writes and `queryExecute()` calls share one transaction.

## Fixes

* Dynamic finders with `InList` / `NotInList` bound the raw list string when the compiled HQL came from the cache.
* Dynamic finder HQL is compiled once per entity and method, and cached.
