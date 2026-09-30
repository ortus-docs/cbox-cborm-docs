---
description: Quickly install cborm
icon: download
---

# Installation

Leverage CommandBox to install into your ColdBox app:

```bash
# Latest version
install cborm

# Bleeding Edge
install cborm@be
```

## System Requirements

* BoxLang 1.17.5+
* The [bx-orm](https://forgebox.io/view/bx-orm) module 2.x (Hibernate 7.4)
* ColdBox 8+
* CFML applications: the `bx-compat-cfml` module, so your `.cfc` code runs on BoxLang

{% hint style="warning" %}
cborm 6 is a pure BoxLang module. Adobe ColdFusion and Lucee are **not** supported: stay on the cborm 5.x series for those engines. See [Upgrading to 6](../intro/release-history/upgrading-to-6.md).
{% endhint %}

Install the ORM module into your BoxLang runtime or server:

```bash
install bx-orm
```

## Application Setup

Enable the ORM in your `Application.bx` (or `Application.cfc`) and point it at the cborm event handler. If you are using the ORM event handler, `ActiveEntity` or any ColdBox proxies that require ORM, you must also create an application mapping to the module, because the ORM starts before ColdBox registers its module paths.

{% code title="Application.bx" %}
```javascript
class {

    this.name = "MyApp";

    // cborm mapping: the ORM boots before ColdBox registers module paths
    this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";

    this.datasource  = "myDatasource";
    this.ormEnabled  = true;
    this.ormSettings = {
        // Where your entities live
        entityPaths    : [ "models" ],
        dbcreate       : "update",
        // Let cborm announce ORM events to ColdBox interceptors and autowire entities
        eventHandling  : true,
        eventHandler   : "cborm.models.EventHandler",
        // Let ColdBox or your services manage flushing
        flushAtRequestEnd : false
    };

}
```
{% endcode %}

{% hint style="info" %}
`cborm.models.BXEventHandler` still works as a deprecated alias of `cborm.models.EventHandler`. Every other ORM setting is documented in the bx-orm module's documentation.
{% endhint %}

## WireBox DSL

The module registers a new WireBox DSL called `entityservice` which can produce virtual or base ORM entity services. Below are the injections you can use:

* `entityservice` -  Inject a global ORM service
* `entityservice:{entityName}` - Inject a Virtual entity service according to `entityName`

## Module Settings

Here are the module settings you can place in your `ColdBox.cfc` under `moduleSettings` -> `cborm` structure or by creating a `cborm.cfc` in the `config/modules` directory.

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

ColdBox 7+ Config:

{% tabs %}

{% tab title="BoxLang" %}
{% code title="config/modules/cborm.bx" lineNumbers="true" %}

```javascript
class{
  function configure(){
     return {
        // Resource Settings
        resources : {
            // Enable the ORM Resource Event Loader
            eventLoader : false,
            // Pagination max rows
            maxRows : 25,
            // Pagination max row limit: 0 = no limit
            maxRowsLimit : 500
        },
            // WireBox Injection bridge
            injection : {
                // enable entity injection via WireBox
                enabled : true,
                // Which entities to include in DI ONLY, if empty include all entities
                include : "",
                // Which entities to exclude from DI, if empty, none are excluded
                exclude : ""
            }
      }
    }
}
```

{% endcode %}
{% endtab %}

{% tab title="CFML" %}
{% code title="config/modules/cborm.cfc" lineNumbers="true" %}

```javascript
component{
  function configure(){
     return {
        // Resource Settings
        resources : {
            // Enable the ORM Resource Event Loader
            eventLoader : false,
            // Pagination max rows
            maxRows : 25,
            // Pagination max row limit: 0 = no limit
            maxRowsLimit : 500
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
      };
    }
}
```

{% endcode %}
{% endtab %}

{% endtabs %}

## Validation

We have also integrated a `UniqueValidator` from the **validation** module into our ORM module. It is mapped into WireBox as `UniqueValidator@cborm` so you can use it in your model constraints like so:

```javascript
{ fieldName : { validator: "UniqueValidator@cborm" } }
```

## Hibernate Version

Hibernate is bundled with the `bx-orm` module. cborm 6 runs on `bx-orm` 2, which bundles Hibernate ORM 7.4: [https://hibernate.org/orm/documentation/7.4/](https://hibernate.org/orm/documentation/7.4/)

{% hint style="info" %}
**Automatic value conversion**: bx-orm converts values to the Java types Hibernate expects, so cborm no longer casts values itself. `idCast()` and `autoCast()` remain for compatibility and do not cast.
{% endhint %}
