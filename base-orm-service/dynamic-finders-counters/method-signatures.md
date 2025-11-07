# Method Signatures

We have three types of dynamic finders and counters:

* `findBy` : Find ONE entity according to method signature, if more than one record is found an exception is thrown
* `findAllBy` : Find ALL entities according to method signature
* `countBy` : Give you a count of entities according to method signature

Let's say you have the following entity:

```javascript
component persistent="true" name="User" extends="cborm.models.ActiveEntity"{

     property name="id" column="user_id" fieldType="id" generator="uuid";
     property name="lastName";
     property name="userName";
     property name="password";
     property name="lastLogin" ormtype="date";
}
```

Then we could do the following:

```javascript
user = getInstance( "User" ).findByLastName( "Majano" );

users = getInstance( "User" ).findAllByLastNameLike( "Ma%" );

users = getInstance( "User" ).findAllByLastLoginBetween( "01/01/2010", "01/01/2012" );

users = getInstance( "User" ).findAllByLastLoginGreaterThan( "01/01/2010" );

users = getInstance( "User" ).findAllByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );

count = getInstance( "User" ).countByLastLoginGreaterThan( "01/01/2010" );

count = getInstance( "User" ).countByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );
```

You can also use the virtual entity service instead of active entity.

```javascript
// Get a virtual entity service via DI, there are many ways to get a virtual entity service
// Look at virtual entity service docs for retrieval
property name="userService" inject="entityservice:userService";

user = userService.findByLastName( "Majano" );

users = userService.findAllByLastNameLike( "Ma%" );

users = userService.findAllByLastLoginBetween( "01/01/2010", "01/01/2012" );

users = userService.findAllByLastLoginGreaterThan( "01/01/2010" );

users = userService.findAllByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );

count = userService.countByLastLoginGreaterThan( "01/01/2010" );

count = userService.countByLastLoginGreaterThanAndLastNameLike( "01/01/2010", "jo%" );
```

If you just use a vanilla Base ORM Service, then the first argument must be the `entityName`:

```javascript
// Get a virtual entity service via DI, there are many ways to get a base entity service
// Look at base entity service docs for retrieval
property name="userService" inject="entityservice";

user = userService.findByLastName( "User", "Majano" );
```
