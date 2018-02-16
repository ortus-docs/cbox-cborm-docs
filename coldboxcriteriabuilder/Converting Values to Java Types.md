#Converting Values to Java Types
By default you will need to do some javaCasting() on the values in order for the criteria builder to work correctly on some values. Remember that ColdFusion is a typeless language and Java is not. However, we have added to convenience methods for you so you can just pass in values without caring about casting:

* convertIDValueToJavaType(id)
* convertValueToJavaType(propertyName, value)

You can also find these methods in the Base ORM services and Virtual Entity Services.

```javascript
c.convertIDValueToJavaType( id = 123 );

c.convertIDValueToJavaType( id = ["1","2","3"] );

c.convertValueToJavaType(propertyName="id", value=arguments.testUserID)
```

