---
description: ColdBox ORM Module
---

# Introduction

The `cborm` module is a module that will **enhance** your experience when working with the ColdFusion ORM powered by [Hibernate](https://hibernate.org/). It will not only enhance it with dynamic goodness but give you a fluent and human approach to working with Hibernate. Basically making working with Hibernate not SUCK!

![](.gitbook/assets/hibernate-logo.svg)

### Some Features

* **Service Layers** with all the methods you could probably think off to help you get started in any project
* **Virtual service layers** so you can create virtual services for any entity in your application
* Automatic RESTFul resources handler, focus on your domain objects and business logic, not the boilerplate of REST
* `ActiveEntity` our implementation of Active Record for ORM
* Fluent queries via Hibernate's criteria and detached criteria queries with some Dynamic CFML goodness
* Automatic transaction demarcation for save and delete operations
* Dynamic finders and counters for expressive and fluent shorthand SQL
* Automatic Java casting
* Entity population from json, structs, xml, and queryies including building up their relationships
* Entity validation via [cbValidation](https://forgebox.io/view/cbvalidation)
* Includes the [Mementifier project](https://www.forgebox.io/view/mementifier) to produce memento states from any entity, great for producing JSON
* Ability for finders and queries to be returned as Java streams using our [cbStreams](https://www.forgebox.io/view/cbstreams) project.

```javascript
# A quick preview of some functionality

var book = new Book().findByTitle( "My Awesome Book" );
var book = new Book().getOrFail( 2 );
new Book().getOrFail( 4 ).delete();
new Book().deleteWhere( isActive:false, isPublished:false );

property name="userService" inject="entityService:User";

return userService.list();
return userService.list( asStream=true );

var count = userService.countWhere( age:20, isActive:true );
var users = userService.findAllByLastLoginBetween( "01/01/2019", "05/01/2019" );

userService
    .newCriteria()
    .eq( "name", "luis" )
    .isTrue( "isActive" )
    .getOrFail();

userService
    .newCriteria()
    .isTrue( "isActive" )
    .joinTo( "role" )
        .eq( "name", "admin" )
    .asStream()
    .list();

userService
    .newCriteria()
    .withProjections( property="id,fname:firstName,lname:lastName,age" )
    .isTrue( "isActive" )
    .joinTo( "role" )
        .eq( "name", "admin" )
    .asStruct()
    .list();
```



**In other words, it makes using an ORM not SUCK!**

## Versioning

The ColdBox ORM Module is maintained under the [Semantic Versioning](http://semver.org) guidelines as much as possible.Releases will be numbered with the following format:

```text
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major \(and resets the minor and patch\)
* New additions without breaking backward compatibility bumps the minor \(and resets the patch\)
* Bug fixes and misc changes bumps the patch

## License

Apache 2 License: [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

## Important Links

* **Code**: [https://github.com/coldbox-modules/cbox-cborm](https://github.com/coldbox-modules/cbox-cborm)
* **Issues**: [https://ortussolutions.atlassian.net/browse/CBORM](https://ortussolutions.atlassian.net/browse/CBORM)
* **ForgeBox**: [https://forgebox.io/view/cborm](https://forgebox.io/view/cborm)

## Discussion & Help

![](.gitbook/assets/image%20%282%29.png)

The Ortus Community is the way to get any type of help for our entire platform and modules: [https://community.ortussolutions.com](https://community.ortussolutions.com)

## Professional Open Source

![Ortus Solutions, Corp](.gitbook/assets/ortussolutions_button.png)

The ColdBox ORM Module is a professional open source software backed by [Ortus Solutions, Corp](http://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### HONOR GOES TO GOD ABOVE ALL

Because of His grace, this project exists. If you don't like this, then don't read it, it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

