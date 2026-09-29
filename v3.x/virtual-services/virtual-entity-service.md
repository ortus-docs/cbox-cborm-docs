# Overview

The virtual entity service is another support class that can help you create **virtual** service layers that are bounded to a specific ORM entity for convenience. This class inherits from our Base ORM Service and allows you to do everything the base class provides, except you do not need to specify to which `entityName` or `entity` you are working with.&#x20;

![](https://github.com/ColdBox/cbox-cborm/wiki/VirtualEntityService.jpg)

You can also use this class as a base class and template out its methods to more concrete usages. The idea behind this virtual entity service layer is to allow you to have a very nice abstraction to all the CF ORM capabilities (hibernate) and promote best practices.

{% hint style="success" %}
**Tip:** Please remember that you can use **ANY** method found in the [Base ORM Service](../getting-started/overview.md#base-orm-service) except that you will not pass an argument of `entityName` anymore as you have now been bounded to that specific entity.
{% endhint %}

## Injection Virtual Services

The WireBox injection DSL has an injection namespace called `entityService` that can be used to wire in a Virtual Entity Service bound to ANY entity in your application. You will use this DSL in conjunction with the name of the entity to manage it.

| Inject Content           | Description                               |
| ------------------------ | ----------------------------------------- |
| `entityService:{entity}` | A virtual service based on the `{entity}` |

```javascript
component{
    // Virtual service layer based on the User entity
    property name="userService" inject="entityService:User";
    
}
```

## Requesting Virtual Services

You can also request a virtual service via the `getInstance()` method in your handlers or view a `wirebox.getInstance()` call:

```java
userService = getInstance( dsl="entityService:User" );

usersFound = userService.count();
user = userService.new( {firstName="Luis",lastName="Majano",Awesome=true} );
userService.save( user );

user = userService.get( "123" );

var users = userService.newCriteria()
    .like("lastName", "%maj%")
    .isTrue("isActive")
    .list(sortOrder="lastName");
```

## Mapping Virtual Services

You can also leverage the WireBox Binder to map your virtual services so you can abstract the object's constructions or add constructor arguments to their definition and have full control:

{% code title="config/WireBox.cfc" %}
```java
function configure(){
    
    map( "UserService" )
        .to( "cborm.models.VirtualEntityService" )
        .initArg( name="entityName", value="User" )
        .initArg( name="useQueryCaching", value="true" )
        .initArg( name="defaultAsQuery", value="false" );
}
```
{% endcode %}

Now you can just use it via the `UserService` alias:

{% code title="handlers/user.cfc" %}
```java
component{
    
    // Inject Alias
    property name="userService" inject="UserService";
    
    function index( event, rc, prc ){
        var usersFound = userService.count();
        var user = userService.new( {firstName="Luis",lastName="Majano",Awesome=true} );
        userService.save( user );
        
        var user = userService.get( "123" );
        
        var users = userService.newCriteria()
            .like("lastName", "%maj%")
            .isTrue("isActive")
            .list(sortOrder="lastName");
    }

}
```
{% endcode %}
