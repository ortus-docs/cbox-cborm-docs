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

* BoxLang 1.0+
* Adobe ColdFusion 2023+
* Lucee 5.x+
* Hibernate 5.x+ (Hibernate 3 is no longer supported as of CBORM 4.9.0)

{% hint style="warning" %}
**Breaking Change**: CBORM 4.9.0 dropped support for Hibernate 3. All supported CFML engines must use Hibernate 5.x or later. If you're using an older CFML engine with Hibernate 3, you must upgrade before using CBORM 4.9.0+.
{% endhint %}

## Application Setup

If you are using the ORM EventHandler or `ActiveEntity` or any ColdBox Proxies that require ORM, you must create an Application Mapping to the module in the `Application.bx|cfc` like this:

{% code title="Application.bx|cfc" %}
```javascript
# In the pseudo constructor
this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";
```
{% endcode %}

This is required because the ORM engine is bootstraped before ColdBox fully initializes and thus the module paths are not yet registered.

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

## Supported Hibernate Versions

### BoxLang 1.0+

Hibernate is bundled with the `bx-orm` module.  Just install it via CommandBox:

```bash
install bx-orm
```

The version of Hibernate bundled is:

* Hibernate 5.6+ - [https://hibernate.org/orm/documentation/5.6/](https://hibernate.org/orm/documentation/5.6/)

{% hint style="info" %}
**BoxLang Autocasting**: As of CBORM 4.8.0, CBORM leverages BoxLang's intelligent autocasting capabilities. When running on BoxLang, CBORM passes values through directly to take advantage of BoxLang's smarter type handling, improving performance and type safety.
{% endhint %}

### Lucee 5+

Hibernate is bundled with the Ortus ORM Extension for Lucee: https://forgebox.io/view/D062D72F-F8A2-46F0-8CBC91325B2F067B.  You can find our source code here: https://github.com/ortus-solutions/extension-hibernate

You can install it via the Lucee Administrator, by using th extension ID: `D062D72F-F8A2-46F0-8CBC91325B2F067B` or via CommandBox:

```bash
box install D062D72F-F8A2-46F0-8CBC91325B2F067B
```

BY JVM argument

```bash
-Dlucee-extensions=D062D72F-F8A2-46F0-8CBC91325B2F067B
```

The version of Hibernate bundled is:

```bash
-Dhibernate.version=5.6.0.Final
```

* Hibernate 5.6+ - [https://hibernate.org/orm/documentation/5.6/](https://hibernate.org/orm/documentation/5.6/)

> Please note that our Lucee Hibernate Extension is on LTS and will only receive critical bug fixes and security patches.  New features and enhancements will be focused on the BoxLang BX-ORM module.

### Adobe 2023+

* Hibernate 5.2+ - [https://hibernate.org/orm/documentation/5.2/](https://hibernate.org/orm/documentation/5.2/)
