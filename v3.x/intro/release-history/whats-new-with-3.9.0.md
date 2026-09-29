---
description: August 10, 2022
---

# What's New With 3.9.0

### Added

* New `when( boolean, success, fail )` fluent construct for `ActiveEntity`, `VirtualEntityService` and the `BaseORMService` to allow for fluent chaining of operations on an entity or its service.
* Migration to new ColdBox Virtual App Testing approaches
* Removed unnecessary on load logging to increase performance
* Hibernate 5.4 on Lucee experimental testing

### Fixed

* `countWhere()` invalid SQL exception if no arguments are provided: [#54](https://github.com/coldbox-modules/cborm/pull/54)
