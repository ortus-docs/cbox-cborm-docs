# Enabling Active Entity

To work with Active Entity you must do a few things to tell ColdBox you want to use Active Entity and enable validation and injections:

1. Enable the ORM in your Application.cfc with event handling turned on, manage session and flush at request end as false.
2. Enable the orm configuration structure in your [ConfigurationCFC](http://wiki.coldbox.org/wiki/ConfigurationCFC.cfm) to allow for ColdBox to do entity injections

