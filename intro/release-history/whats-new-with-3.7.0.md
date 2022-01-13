# What's New With 3.7.0

### Added

* [CBORM-29](https://ortussolutions.atlassian.net/browse/CBORM-29) Allow SQL and SQL Group projections to be functions containing commas

Example:

```javascript
r = categoryCriteria
.withProjections(
	groupProperty = "catid",
	sqlProjection = [
		{
			sql      : "count( category_id )",
			alias    : "count",
			property : "catid"
		}
	],
	sqlGroupProjection = [
		{
			sql      : "year( modifydate )",
			group    : "year( modifydate )",
			alias    : "modifiedDate",
			property : "id"
		},
		{
			sql      : "dateDiff('2021-12-31 23:59:59','2021-12-30')",
			group    : "dateDiff('2021-12-31 23:59:59','2021-12-30')",
			alias    : "someDateDiff",
			property : "id"
		}
	]
)
.asStruct()
.peek( function( c ){
	debug( c.getSql( true, true ) );
} )
.list();

debug( r );
assertTrue( isArray( r ) );
```
