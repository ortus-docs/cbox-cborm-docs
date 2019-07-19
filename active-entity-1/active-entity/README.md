# Active Entity

This class allows you to simulate the Active Record pattern in your ORM entities by inheriting from our Active Entity class. This will make your ORM entities get all the functionality of our Virtual and Base ORM services so you can do finds, searches, listings, counts, execute queries, transaction safe deletes, saves, updates, criteria building, and even [validation](validation.md) right from within your ORM Entity. The idea behind the Active Entity is to allow you to have a very nice abstraction to all the ColdFusion ORM capabilities \(hibernate\) and all of our ORM extensions like our ColdBox Criteria Builder. With Active Entity you will be able to:

* Find entities using a variety of filters and conditions
* ORM paging
* Specify order, searches, criterias and grouping of orm listing and searches
* Use DLM style hibernate operations for multiple entity deletion, saving, and updating
* Check for existence of records
* Check for counts using criterias
* Use our extensive ColdBox Criteria Builder to build Object Oriented HQL queries
* Validate your entity using our awesome [validation engine](https://github.com/ColdBox/cbox-validation/wiki)

