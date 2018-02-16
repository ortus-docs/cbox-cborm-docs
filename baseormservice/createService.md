#createService
Create a virtual service for a specfic entity. Basically a new service layer that inherits from the BaseORMService object but no need to pass in entity names, they are bound to the entity name passed here.

###Returns

* This function returns *VirtualEntityService*

###Arguments

| Key | Type | Required | Default |
| --- | --- | --- | --- |
| entityName | string | Yes | --- |
| useQueryCaching | boolean | No | Same as BaseService |
| queryCacheRegion | string | No | Same as BaseService |

###Examples

```javascript
userService = ormService.createService("User");
userService = ormService.createService("User",true);
userService = ormService.createService("User",true,"MyFunkyUserCache");

// Remember you can use virtual entity services by autowiring them in via our DSL
component{
  property name="userService" inject="entityService:User";
  property name="postService" inject="entityService:Post";	
}
```
