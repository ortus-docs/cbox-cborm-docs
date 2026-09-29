---
description: December 16, 2021
---

# What's New With 3.5.0

### Fixed

[CBORM-20](https://ortussolutions.atlassian.net/browse/CBORM-20) ActiveEntity `evict()` had the wrong method and arguments delegated to the parent class.

[CBORM-9](https://ortussolutions.atlassian.net/browse/CBORM-9) ACF2021 - `org.hibernate.SessionFactory.getAllClassMetadata` is no longer supported

### Improved

[CBORM-14](https://ortussolutions.atlassian.net/browse/CBORM-14) Inline datasource discovery in base orm service to get a performance boost

[CBORM-13](https://ortussolutions.atlassian.net/browse/CBORM-13) virtual entity service double creating the orm utility, use the parent one instead of duplicating the effort

[CBORM-12](https://ortussolutions.atlassian.net/browse/CBORM-12) Lazy load the `getORMUtil()` and use it only when required.

### Added

[CBORM-22](https://ortussolutions.atlassian.net/browse/CBORM-22) New orm util support method: `setupHibernateLogging()` thanks to michael born

[CBORM-19](https://ortussolutions.atlassian.net/browse/CBORM-19) Added a `isInTransaction()` util helper method to all the orm services.

[CBORM-18](https://ortussolutions.atlassian.net/browse/CBORM-18) New ORM events based on Hibernate 5.4 Events: `ORMFlush, ORMAutoFlush, ORMPreFlush, ORMDirtyCheck, ORMEvict, and ORMClear`

[CBORM-17](https://ortussolutions.atlassian.net/browse/CBORM-17) Hibernate 5.4 support for lucee new extension

[CBORM-16](https://ortussolutions.atlassian.net/browse/CBORM-16) Adobe 2021 support and testing automations

[CBORM-15](https://ortussolutions.atlassian.net/browse/CBORM-15) Migration to github actions

[CBORM-11](https://ortussolutions.atlassian.net/browse/CBORM-11) Allow Criteria Builder `Get()` and `getOrFail()` Methods to Return Projection List Properties

[CBORM-21](https://ortussolutions.atlassian.net/browse/CBORM-21) New cfformating rules

## Compatibility

* If you upgrade your lucee ORM extension to use Hibernate 5.4, all positional paramters in HQL using `?` has been deprecated. You will have to use the `?x` approach where `x` is a number according to the position in the sql:

```sql
// Old Syntax
select p 
from Person p 
where p.name like ? and p.isStatus = ?

// New Syntax
select p 
from Person p 
where p.name like ?1 and p.isStatus = ?2
```
