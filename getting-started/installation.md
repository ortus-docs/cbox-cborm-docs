# Installation

Leverage CommandBox to install into your ColdBox app:

```bash
# Latest version
install cborm

# Bleeding Edge
install cborm@be
```

### System Requirements

* Lucee 5.x+&#x20;
* ColdFusion 2016+

## Application.cfc Setup

Unfortunately, due to the way that ORM is loaded by ColdFusion, if you are using the ORM EventHandler or `ActiveEntity` or any ColdBox Proxies that require ORM, you must create an Application Mapping in the `Application.cfc` like this:

{% code title="Application.cfc" %}
```javascript
# In the pseudo constructor
this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";
```
{% endcode %}

## WireBox DSL

The module registers a new WireBox DSL called `entityservice` which can produce virtual or base ORM entity services. Below are the injections you can use:

* `entityservice` -  Inject a global ORM service
* `entityservice:{entityName}` - Inject a Virtual entity service according to `entityName`

## Module Settings

Here are the module settings you can place in your `ColdBox.cfc` under `moduleSettings` -> `cborm` structure:

{% code title="config/ColdBox.cfc" %}
```javascript
moduleSettings = {
    cborm = {
        // Resource Settings
    		resources : {
    			// Enable the ORM Resource Event Loader
    			eventLoader 	: false,
    			// Pagination max rows
    			maxRows 		: 25,
    			// Pagination max row limit: 0 = no limit
    			maxRowsLimit 	: 500
    		},
        // WireBox Injection bridge
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

## Validation

We have also integrated a `UniqueValidator` from the **validation** module into our ORM module. It is mapped into WireBox as `UniqueValidator@cborm` so you can use in your model constraints like so:

```javascript
{ fieldName : { validator: "UniqueValidator@cborm" } }
```

## Supported Hibernate Versions

### Lucee 5

* Hibernate 3.5 - [https://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html](https://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html)
* Hibernate 5.4 - [https://hibernate.org/orm/documentation/5.4/](https://hibernate.org/orm/documentation/5.4/)
  * You will need to update to the latest ORM Beta Extension - [https://download.lucee.org/#FAD1E8CB-4F45-4184-86359145767C29DE](https://download.lucee.org/#FAD1E8CB-4F45-4184-86359145767C29DE)
  * [**5.4.29.6-BETA (Aug 6, 2021)**](https://ext.lucee.org/hibernate-orm-5.4.29.6-BETA.lex)

### Adobe 2016

* Hibernate 4.3 - [https://hibernate.org/orm/documentation/4.3/](https://hibernate.org/orm/documentation/4.3/)

### Adobe 2018, Adobe 2021

* Hibernate 5.2 - [https://hibernate.org/orm/documentation/5.2/](https://hibernate.org/orm/documentation/5.2/)
