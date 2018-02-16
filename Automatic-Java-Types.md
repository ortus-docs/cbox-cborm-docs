#Automatic Java Types
Most of the Hibernate extensions like criteria builders and even some dynamic finders and counters will have to rely on the underlying Java type in order to work. You do this in ColdFusion by using the `javaCast()` function available to you. So if you are using a primary key that is an `Integer` you might have to do the following in order to match your variable to the underlying Java type:

```javascript
criteria.eq( "id", javaCast( "int", arguments.id ) );
```

If you do not type it, then ColdFusion assumes it is a string and passes a string to Hibernate which will throw an exception as it is supposed to be an integer. 

## Auto Types
We have created two methods available to you in the base orm service, virtual service, criteria builders, active entity, etc to help you with these translations by automatically casting the values for you:

```js
// Coverts a value to the correct javaType for the property passed in The method returns the value in the proper Java Type
any convertValueToJavaType( any propertyName, any value )

// Coverts an ID, list of ID's, or array of ID's values to the proper java type The method returns a coverted array of ID's
any convertIDValueToJavaType( any id )
```

So instead of casting it manually you can just let us do the work:

```javascript
criteria.eq( "id", criteria.convertIDValueToJavaType( arguments.id ) );
```

