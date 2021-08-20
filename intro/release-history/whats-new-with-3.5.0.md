# What's New With 3.5.0

## \[v3.5.0\] =&gt; 2021-AUG

### Added

* Migration to github actions from travis
* Adobe 2021 Support and testing automation
* Hibernate 5.4+ support for Lucee
* New ORM events based on Hibernate 5.4 Events: `ORMFlush, ORMAutoFlush, ORMPreFlush, ORMDirtyCheck, ORMEvict, and ORMClear`
* Added a `isInTransaction()` util helper method to all the orm services. This adds the ability to check whether the current executing code is inside a Hibernate transaction. Useful for preventing nested transactions via

### Fixed

* ActiveEntity `evict()` had the wrong method and arguments delegated to the parent class.

### Compatibility

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

