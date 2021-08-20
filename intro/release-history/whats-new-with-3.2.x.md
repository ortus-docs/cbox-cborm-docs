---
description: 'March 31, 2021'
---

# What's New With 3.2.x

### Added

* Exposed a `getSQLHelper()` from criterias to allow for usage of formmatting of sql
* New interception points: `beforeOrmExecuteQuery, afterOrmExecuteQuery` from the base orm service: `executeQuery()` method

### Fixed

* Moved `afterCriteriaBuilderList` event before results conversions



## \[v3.2.1\] =&gt; 2021-MAR-31

### Fixed

* Wrong object to get the event handler manager when doing execute query calls

