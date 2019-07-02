# Entity Injection Capabilities

By enabling the global event handler, [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm) can also be enabled to listen to these events and use them to wire up dependencies in these ORM entities and thus provide dependency injection for ORM entities. The injection takes place during the postLoad\(\) and postNew\(\) event cycles. All you need to do is enable ORM injection via the [ConfigurationCFC](http://wiki.coldbox.org/wiki/ConfigurationCFC.cfm)

> **Important** If you have to also override the event handler's _postLoad\(\)_,_postNew\(\)_ method, you will then need to call the super class method so the injection procedures can take place: _super.postLoad\(entity\)_ _super.postNew\(entity\)_

The video below describes the entire process:

![](https://raw.githubusercontent.com/wiki/coldbox-modules/cbox-cborm/entityInjectionCapabilities.png)

