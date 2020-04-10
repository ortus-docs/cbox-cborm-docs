# What's New With 2.5.0

## Release Notes

* `Features` : Introduction of the automatic resource handler for ORM Entities based on ColdBox's 6 resources and RestHandler
* `Improvement` : Natively allow for nested transactions and savepoints by not doing preemptive transaction commits when using transactions.
* `Bug` : Fix on `getOrFail()` where if the id was 0, it would still return an empty object.
* `Task` : Added formatting via cfformat

You can read more about the [RESTFul Resources here](../../orm-events/automatic-rest-crud.md):

