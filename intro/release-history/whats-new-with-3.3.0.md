# What's New With 3.3.0

### Added

* New `eventPrefix` setting so you can prefix the resource REST CRUD events with whatever you like.
* Useful exceptions when `results` struct does not have the required keys
* Ability to override the name of the method to use for persistence on the ORM services. Using the `variables.saveMethod` property or the `savemethod` argument.
* Ability to override the name of the method to use for deleting entities on the ORM services. Using the `variables.deleteMethod` property or the `deleteMethod` argument.
* cbSwagger docs added to the base resource handler

### Changed

* Added ACF2016 compatibilities on elvis operator which sucks on ACF2016
* Avoid using member function son some arrays to allow for working with Java arrays

