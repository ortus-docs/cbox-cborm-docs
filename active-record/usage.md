# Usage

Now that you have created your entities, how do we use them? Well, you will be using them from your handlers or other services by leveraging WireBox's `getInstance()` method.  You can use entityNew\(\) as well, but if you do, you will loose any initial dependency injection within the entity. This is a ColdFusion limitation where we can't listen for new entity constructions.  If you want to leverage DI, as best practice retrieve everything from WireBox.

Once you have an instance of the entity, then you can use it to satisfy your requirements with the entire gamut of functions available from the [base services](../base-orm-service-1/service-methods/).

{% hint style="success" %}
**Tip:** You can also check out our [Basic CRUD](../getting-started/basic-crud.md) guide for an initial overview of the usage.
{% endhint %}

```javascript
component{
    
    function index( event, rc, prc ){
        prc.data = getInstance( "User" ).list( sortOrder="fname" );
        prc.stream = getInstance( "User" ).list( sortOrder="fname", asStream=true );
    }
    
    function count( event, rc, prc ){
        return getInstance( "User" ).count()
        return getInstance( "User" ).countWhere( { isActive : true } );
    }
    
    function show( event, rc, prc ){
        return getInstance( "User" )
            .getOrFail( 123 )
            .getMemento();
    }
    
    function save( event, rc, prc ){
        return populateModel( model="User", composeRelationships=true )
            .save()
            .getMemento();
    }
    
    function delete( event, rc, prc ){
        getInstance( "User" )
            .getOrFail( rc.id ?: -1 )
            .delete();
        return "User Deleted";
    }

}
```

