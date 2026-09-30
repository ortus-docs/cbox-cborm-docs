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

## Native Java Streams

`asStream = true` on `list()`, `executeQuery()`, `findAll()`, `findAllWhere()`, `getAll()` and the dynamic finders, and criteria `asStream()`, return native Java streams read from the database as they are consumed. cbStreams is no longer a dependency. See [Java Streams](../../advanced/java-streams.md).

## New Service Methods

| Method                                                                               | What it does                                                         |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| [findWhereOrFail](../../base-orm-service/service-methods/finders/findwhereorfail.md) | `findWhere()`, or an `EntityNotFound` exception                      |
| [firstOrNew](../../base-orm-service/service-methods/finders/firstornew.md)           | The first match, or a new unsaved entity populated with the criteria |
| [firstOrCreate](../../base-orm-service/service-methods/finders/firstorcreate.md)     | The first match, or a new entity populated and saved                 |
| [updateWhere](../../base-orm-service/service-methods/saving-entities/updatewhere.md) | One bulk update for the entities matching a criteria struct          |
| [getReference](../../base-orm-service/service-methods/getters/getreference.md)       | A lazy reference to an entity, without a `SELECT`                    |
| [lock](../../base-orm-service/service-methods/orm-session/lock.md)                   | Lock an entity's row until the transaction ends                      |
| [readOnly](../../base-orm-service/service-methods/orm-session/readonly.md)           | Run a closure with every entity it loads read-only                   |

`get()`, `getOrFail()`, `list()`, `executeQuery()`, `findAll()`, `findWhere()` and `findAllWhere()` also take an `options` struct passed to bx-orm: `readOnly`, `lock`, `uniqueFirst`, `fetchSize`, `comment`, `hints` and more.

`ActiveEntity` adds `lock()` and `toStruct()`, and `getKeyValue()`, `getDirtyPropertyNames()` and `sessionContains()` default to the entity itself.

## Safer, Sturdier Helpers

* `exists()`, `countWhere()`, `deleteWhere()`, `deleteByID()`, `deleteAll()` and `getAll()` run on bx-orm criteria queries: property names are validated, bulk deletes flush pending changes first, and composite ids work (pass a struct of key values).
* `getAll( sortOrder )` and the dynamic finder `sortBy` option only accept property names with `asc`/`desc`, so they can never inject HQL.
* `save()`, `delete()` and `saveAll()` flush every datasource their entities belong to.
* The `UniqueValidator` uses an `EXISTS` query and supports composite ids.
* Dynamic finders read property names (id properties included) from bx-orm's metadata, keep bx-orm's typed errors with their "Did you mean" hints, and recompile themselves when an `ormReload()` changes an entity.

## Mementifier and Pagination

Mementifier stays a cborm dependency and the default way the resource handler marshals entities. Entities without `getMemento()` fall back to bx-orm's `entityToStruct()`. The resource handler builds its own `pagination` block, so cbPaginator is no longer a dependency.

## Case-Insensitive Names

HQL entity and property names resolve in any case, as the rest of BoxLang does: `executeQuery( "from user where firstname = ?", [ "Luis" ] )` finds the `User` entity's `firstName` property. Criteria queries and dynamic finders (including their `sortBy` option) accept any case too.

## Events

* New `ORMPostCommit` interception point, announced once an insert, update or delete is committed. Data: `entity`, `entityName`, `action`.
* `ORMPostNew` is announced once per `new()`, after the entity is autowired and populated. `entityNew()`, `entityLoadOrNew()` and `entityLoadOrSave()` autowire the entity and announce it too.
* `ORMPreFlush` is announced on every flush, from bx-orm's flush events.
* One event handler: `cborm.models.EventHandler`. `BXEventHandler` remains as a deprecated alias.
* The bx-orm criteria events (`onCriteriaBuilderAddition`, `beforeCriteriaBuilderList`, ...) are relayed to your ColdBox interceptors.

## Transactions

Service transactions and the `HibernateTransaction` aspect ride BoxLang `transaction{}` blocks, so ORM writes and `queryExecute()` calls share one transaction.

## Fixes

* Dynamic finders with `InList` / `NotInList` bound the raw list string when the compiled HQL came from the cache.
* Dynamic finder HQL is compiled once per entity and method, and cached.
* HQL injection through `getAll( sortOrder )` and the dynamic finder `sortBy`.
* `deleteWhere()` missed entities saved earlier in the request but not flushed yet.
* `delete()` flushed only the first entity's datasource, and `saveAll()` only the service's.
* `VirtualEntityService.deleteAll()` dropped `transactional`, `evictCollection()` returned nothing, and `findAllWhere()` and `new()` were missing arguments of the base service.
* The entity injection `include`/`exclude` lists matched parts of names (`UserRole` included `User`).
* `executeQuery( asQuery = true )` returned an array when the HQL mentioned `update`, `insert` or `delete`.
* `ActiveEntity.getValidationResult()` was always null, and `isValid( includeFields )` ignored `includeFields`.
* A `new()` nested in an entity's `postNew()` announced `ORMPostNew` twice.
