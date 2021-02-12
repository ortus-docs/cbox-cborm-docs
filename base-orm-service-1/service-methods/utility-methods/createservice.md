# createService

Create a virtual service for a specific entity. Basically a new service layer that inherits from the `BaseORMService` object but no need to pass in entity names, they are bound to the entity name passed here.

## Returns

* This function returns _VirtualEntityService_

## Arguments

| Key | Type | Required | Default |
| :--- | :--- | :--- | :--- |
| entityName | string | Yes | --- |
| useQueryCaching | boolean | No | Same as BaseService |
| queryCacheRegion | string | No | Same as BaseService |
| eventHandling | boolean | No | true |
| useTransactions | boolean | No | true |
| defaultAsQuery | boolean | No | true |
| datasource | string | No | The default app datasource |

## Examples

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

