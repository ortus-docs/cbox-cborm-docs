---
description: "Active Entity - Implementing Active Record Pattern in CBORM"
icon: users-gear
---

# Active Entity Overview

![](../.gitbook/assets/active-record.jpg)

This class allows you to implement the [Active Record](https://en.wikipedia.org/wiki/Active\_record\_pattern) pattern in your ORM entities by inheriting from our Active Entity class. This will make your ORM entities get all the functionality of our Virtual and Base ORM services so you can do finds, searches, listings, counts, execute queries, transaction safe deletes, saves, updates, criteria building, and even [validation](validation.md) right from within your ORM Entity.

The idea behind the Active Entity is to allow you to have a very nice abstraction to all the BoxLang ORM (`bx-orm`, Hibernate) capabilities and all of our ORM extensions like the Criteria Builder. With Active Entity you will be able to:

* Find entities using a variety of filters and conditions
* ORM paging
* Specify order, searches, criterias and grouping of orm listing and searches
* Use DML style HQL operations for multiple entity deletion, saving, and updating
* Check for existence of records
* Check for counts using criterias
* Use the [Criteria Builder](../criteria-queries/criteria-builder/README.md) to build Object Oriented HQL queries
* Validate your entity using [cbValidation](http://forgebox.io/view/cbValidation)

## Configuration

To work with Active Entity you must do a few things to tell ColdBox and Hibernate you want to use Active Entity:

1. Enable the ORM in your `Application.bx` (or `Application.cfc`) with event handling turned on, manage session and flush at request end as **false**.  This will allow Hibernate to talk to the cborm event handler.
2. Enable the orm configuration structure in your ColdBox configuration to allow for ColdBox to do entity injections via WireBox.

### Application.bx

The following are vanilla configurations for enabling the `bx-orm` ORM:

{% code title="Application.bx" %}
```javascript
class {

    // cborm mapping: the ORM starts before ColdBox registers the module
    this.mappings[ "/cborm" ] = getDirectoryFromPath( getCurrentTemplatePath() ) & "modules/cborm";

    // Enable ORM
    this.ormEnabled       = true;
    // ORM Datasource
    this.datasource          = "contacts";
    // ORM configuration settings
    this.ormSettings      = {
        // Location of your entities, default is your convention model folder
        entityPaths = [ "models" ],
        // Choose if you want ORM to create the database for you or not?
        dbcreate = "update",
        // Log SQL or not
        logSQL = true,
        // Don't flush at end of requests, let Active Entity manage it for you
        flushAtRequestEnd = false,
        // Don't manage session, let Active Entity manage it for you
        autoManageSession = false,
        // Activate ORM events: bx-orm only fires them when this is true
        eventHandling       =  true,
        // The cborm event handler: ColdBox interceptions + WireBox injection
        eventHandler = "cborm.models.EventHandler"
    };
}
```
{% endcode %}

{% hint style="info" %}
`cfclocation` still works as an alias of `entityPaths`, so existing CFML settings keep working. `cborm.models.BXEventHandler` is a deprecated alias of `cborm.models.EventHandler`.
{% endhint %}

### Module Settings

Open your `config/ColdBox.bx` (or `config/ColdBox.cfc`) and either un-comment or add the following settings:

{% code title="config/ColdBox.bx" %}
```javascript
moduleSettings = {
    cborm = {
        injection = {
            // enable entity injection via WireBox
            enabled = true,
            // Which entities to include in DI ONLY, if empty include all entities
            include = "",
            // Which entities to exclude from DI, if empty, none are excluded
            exclude = ""
        }
    }
}
```
{% endcode %}

This enables WireBox dependency injection, which we need for `ActiveEntity` to work with validation and other features.  Check out our [installation](../getting-started/installation.md#module-settings) section if you need a refresher.

## Building Entities

Once your configuration is done we can now focus on building out your Active Entities.  You will do so by creating your entities like normal ORM objects but with two additions:

1. They will inherit from our base class: `cborm.models.ActiveEntity`
2. If you have a constructor then it must delegate to the super class via `super.init()`

{% code title="models/User.bx" %}
```javascript
class persistent="true" table="users" extends="cborm.models.ActiveEntity" {

    property name="id" column="user_id" fieldType="id" generator="uuid";
	property name="firstName";
	property name="lastName";
	property name="userName";
	property name="password";
	property name="lastLogin" ormtype="date";

	function init(){
	   return super.init();
	}

}
```
{% endcode %}

{% hint style="info" %}
Please remember that your entities inherit all the functionality of the base and virtual services.  Except no entity names or datasources are passed around.
{% endhint %}
