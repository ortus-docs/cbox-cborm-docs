# Release History

In this section you will find the major release notes for each version we release under this major version.  If you are looking for the release notes of previous major versions use the version switcher at the top left of this documentation book.

## Version 2.0

A complete rewrite of the module to support a more modern and fluent approach to working with Hibernate/ColdFusion ORM.  In this release we had to support 3 versions of Hibernate: 3 \(Lucee\), 4 \(ACF 2016\) and 5 \(ACF 2018\), which in itself proved to be a gargantuan task.

We also focused on bringing more functional programming aspects to working with collections of entities and even introduced **cbStreams** as part of the cborm module.  This gives you the ability to produce streams out of any method that produces a collection of entities.

We also focused on converting the state of an object graph to a raw ColdFusion data struct as we live in the world of APIs.  We include the **mementifier** module which allows every single entity to have a `getMemento()` method that will convert itself and its relationships to raw CF data constructs so you can take that state and either marshall it to another format \(json,xml,excel\) or audit the state.

## Version 1.0

The first version of the cbORM series that focused on expanding the native ColdFusion ORM methods and exposing much more Hibernate functionality to the CFML world.

