# What's New With 2.3.0

## Release Notes

* `improvement` : In `executeQuery()` Determine if we are in a UPDATE, INSERT or DELETE, if we do, just return the results instead of a stream or query as the result is always numeric, the rows that were altered.
* `bug` : Fixed `asStream` typo on `executeQuery()`
* `bug` : Missing ACF2016 compat on tests

