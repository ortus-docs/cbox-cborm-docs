---
description: March 11, 2022
---

# What's New With 3.8.0

### Fixed

* CBORM-32 - Non-Primary DSN Entities not found. Multi-datasource discovery of entities using virtual services and active entity. This was a regression since version 1.5. This brings back multi-datasource support for active entity, and virtual entity services. [#52](https://github.com/coldbox-modules/cborm/pull/52)
* Detached `Subqueries` was marked as a `singleton` when indeed it was indeed a `transient`. This could have created scoping issues on subquery based detached criteria building.
* Var scoping issues in `BaseBuilder` detached projections
* `DetachedCriteriaBuilder` was not passing the `datasource` to native criteria objects

### Added

* Root `docker-compose.yml` to startup MySQL, or PostgreSQL in docker, for further hacking and testing.
* Java proxy caching to avoid Lucee OSGi issues and increase Java object building performance
* New method in the `BaseOrmService`: `buildJavaProxy()` which leverages our `JavaProxyBuilder` used mostly internally.
* Lazy loading of SQL Helper in criteria queries
* New module template guidelines and CI
* Leverage WireBox aliases for construction of internal objects
* Tons of internal docs and links to hibernate docs
