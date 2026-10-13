---
description: How to upgrade an application from cborm 4 to cborm 5
---

# Upgrading to 5

cborm 5 certifies the module for BoxLang Prime and removes Lucee support. The API is unchanged from the cborm 4.x series, so upgrading is mostly a matter of checking the engine you run on. See [What's New With 5.0.0](whats-new-with-5.0.0.md) for the details.

## Requirements

- Adobe ColdFusion 2023+ or BoxLang 1.0+ (open source, BE or Prime).
- ColdBox 7+.
- Lucee is no longer supported: stay on the cborm 4.x series if you run Lucee.

## What stays the same

- `BaseORMService`, `VirtualEntityService` and `ActiveEntity`: same methods and arguments.
- Dynamic finders and counters (`findBy...`, `findAllBy...`, `countBy...`).
- The criteria builders (`newCriteria()`, the detached criteria builder), restrictions, projections and subqueries.
- The `entityService` WireBox DSL, the `HibernateTransaction` aspect, the resource handler, the `UniqueValidator`, population and validation.
- The ColdBox interception points (`ORMPreLoad`, `ORMPostLoad`, ...).
- The module dependencies: cbvalidation 4, mementifier 3, cbstreams 2 and cbpaginator 2.

## What changed

- **Lucee removed**: Lucee ORM support, in maintenance mode since cborm 4, is gone. Applications running on Lucee must stay on the cborm 4.x series.
- **BoxLang Prime**: cborm is certified and continuously tested against BoxLang Prime and BoxLang BE, in addition to the open-source BoxLang runtime.
