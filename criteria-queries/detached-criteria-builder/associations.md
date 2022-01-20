# Associations

Yes, you can also create associations in Detached Criteria Builder, just like you would with Criteria Builder. Go on, make some uber-complicated queries!

Here we go!...

You can also navigate associations by nesting the criterias using the createCriteria\("association\_name"\) method and then concatenating the properties of the association to query upon. You will basically be switching the pivot point of the query.

```javascript
var c = newCriteria("User");
var users = c.like("name","lui%")
     .createCriteria("admins")
          .like("name","fra%")
     .list();
```

```javascript
var c = newCriteria("User");
var users = c.like("name","lui%")
     .withAdmins().like("name","fra%")
     .list();
```

You can also use a hibernate property approach which aliases the association much how HQL approaches it by using the `joinTo( "associationName", "alias" )` method:

```javascript
var c = newCriteria("User");
var users = c.like("name","lui%")
     .joinTo("admins","a")
     .eq("a.name","Vero")
     .list();
```

The `joinTo()` method accepts a third argument of `joinType`, which can be retrieved from the criteria object:

```javascript
var c = newCriteria( "User" );
var users = c.like( "name", "lui%" )
     .joinTo( "admins", "a", c.RIGHT_JOIN )
     .eq( "a.name", "Vero" )
     .list();
```

Let's see the method signatures for these guys:

```javascript
createCriteria(required string associationName,numeric joinType)
joinTo(required string associationName, required string alias, numeric joinType)
with{AssociationName}( joinType )
```