# createService

Create a virtual service for a specific entity. Basically a new service layer that inherits from the `BaseORMService` object but no need to pass in entity names, they are bound to the entity name passed here.

## Returns

* This function returns _VirtualEntityService_

## Arguments

| Key              | Type    | Required | Default                              |
| ---------------- | ------- | -------- | ------------------------------------ |
| entityName       | string  | Yes      | ---                                  |
| queryCacheRegion | string  | No       | Same as the calling service          |
| useQueryCaching  | boolean | No       | Same as the calling service          |
| eventHandling    | boolean | No       | Same as the calling service          |
| useTransactions  | boolean | No       | Same as the calling service          |
| defaultAsQuery   | boolean | No       | Same as the calling service          |
| datasource       | string  | No       | Same as the calling service          |

## Examples

```javascript
userService = ormService.createService( "User" );
userService = ormService.createService( entityName="User", useQueryCaching=true );
userService = ormService.createService( entityName="User", useQueryCaching=true, queryCacheRegion="MyFunkyUserCache" );

// Remember you can use virtual entity services by autowiring them in via our DSL
class {
  property name="userService" inject="entityService:User";
  property name="postService" inject="entityService:Post";
}
```
