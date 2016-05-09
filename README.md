# Welcome to The ColdBox ORM Module

This module provides you with several enhancements when interacting with the
ColdFusion ORM via Hibernate.  It provides you with virtual service layers,
active record patterns, criteria and detached criteria queries, entity compositions, populations and so much more to make your ORM life easier!

## Main Sections

* [ORM: Base ORM Service](https://github.com/ColdBox/cbox-cborm/wiki/Base-ORM-Service)
* [ORM: Active Entity](https://github.com/ColdBox/cbox-cborm/wiki/Active-Entity)
* [ORM: Virtual Entity Service](https://github.com/ColdBox/cbox-cborm/wiki/Virtual-Entity-Service)
* [ORM: ColdBox ORM Event Handler](https://github.com/ColdBox/cbox-cborm/wiki/ColdBox-ORM-Event-Handler)
* [ORM: ColdBox Criteria Builder](https://github.com/ColdBox/cbox-cborm/wiki/ColdBox-Criteria-Builder)
* [ORM: ColdBox Detached Criteria Builder](https://github.com/ColdBox/cbox-cborm/wiki/ColdBox-Detached-Criteria-Builder)

##SYSTEM REQUIREMENTS
- Lucee 4.5+
- ColdFusion 9.02+

# INSTRUCTIONS
Just drop into your **modules** folder or use the box-cli to install

`box install cborm`

## WARNING

Unfortunately, due to the way that ORM is loaded by ColdFusion, if you are using the ORM EventHandler or ActiveEntity or any ColdBox Proxies that require ORM, you must create an Application Mapping in the `Application.cfc` like this:

```js
this.mappings[ "/cborm" ] = COLDBOX_APP_ROOT_PATH & "modules/cborm";
```

# WireBox DSL
The module also registers a new WireBox DSL called `entityservice` which can produce virtual or base orm entity services:

- `entityservice` -  Inject a global ORM service
- `entityservice:{entityName}` - Inject a Virtual entity service according to `entityName`

# Settings
Here are the module settings you can place in your `ColdBox.cfc` under an `orm` structure:

```js
orm = {
    injection = {
        enabled = true, include = "", exclude = ""
    }
}
```

# Validation
We have also migrated the `UniqueValidator` from the **validation** module into our
ORM module.  It is mapped into wirebox as `UniqueValidator@cborm` so you can use in your constraints like so:

```js
{ fieldName : { validator: "UniqueValidator@cborm" } }
```

---

Please explore this wiki to learn more about our ORM extensions.