#Direct Instantiation
You can also use the virtual entity service by directly instantiating the coldbox.system.orm.hibernate.VirtualEntityService, configuring it and using it:

```javascript
import coldbox.system.orm.hibernate.*

userService = new VirtualEntityService(entityName="User");
userService = new VirtualEntityService(entityName="User",useQueryCaching=true);

usersFound = userService.count();
user = userService.new({firstName="Luis",lastName="Majano",Awesome=true});
userService.save( user );

user = userService.get("123");

var users = userService.newCriteria()
    .like("lastName", "%maj%")
    .isTrue("isActive")
    .list(sortOrder="lastName");
```

