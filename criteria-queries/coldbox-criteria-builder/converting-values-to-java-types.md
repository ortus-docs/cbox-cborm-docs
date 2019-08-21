# Value Casting

We have created several methods available to you in the criteria builders to help you with casting ColdFusion values to Java values so you can use them in Criteria Queries.  Most of the time the engines can do this for us but not always, so we would recommend to just use these convience methods when needed.  

{% hint style="info" %}
Please note that the base services also include these methods with a few differences in arguments.  Just check the API Docs for their usage.
{% endhint %}

### BaseBuilder Casting Methods

### `nullValue()`

Produce a null value that can be used anywhere you like!

### `autoCast( propertyName, value )`

This method allows you to cast any value to the appropriate type in Java for the property passed in.

### `idCast( id )`

This method allows you to cast the identifier value to the appropriate type in Java.

### Example

So instead of casting it manually you can just let us do the work by calling these methods from any of the services.

```javascript
c.eq( "id", c.idCast( arguments.id ) )
    .eq( "timeout", c.autoCast( "timeout", 4000 ) );
```

