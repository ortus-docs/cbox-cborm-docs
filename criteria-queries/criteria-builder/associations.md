# Associations

You can navigate associations in criteria queries in several ways:

* Dot notation for many-to-one relationships ONLY
* Helper methods to create inner criterias or joins: `createCriteria(), joinTo()`

## Dot Notation Navigation

This type of navigation is the easiest but ONLY works with `many-to-one` associations.  Let's say you have a User entity with a Role and the Role has the following properties: `id, name, slug` and you want to get all users that have the role slug of `admin` and are active.  Then you could do this:

```javascript
// Using virtual services
function findAllAdmins(){
     return newCriteria()
          .isTrue( "active" )
          .eq( "role.slug", "admin" )
          .list();
}
```

## Joins

You can also use the `joinTo()` method, which previously was called `createAlias()` to create joins to related associations.  Let's check out the method signature first:

```javascript
/**
 * Join an association, assigning an alias to the joined association
 *
 * You can also use the following alias method : <code>joinTo()</code>
 *
 * @associationName The name of the association property: A dot-separated property path
 * @alias The alias to assign to the joined association (for later reference).
 * @joinType The hibernate join type to use, by default it uses an inner join. Available as properties: criteria.FULL_JOIN, criteria.INNER_JOIN, criteria.LEFT_JOIN
 * @withClause The criterion to be added to the join condition (ON clause)
 */
any function joinTo(
	required string associationName,
	required string alias,
	numeric joinType=this.INNER_JOIN,
	any withClause
)
```

The arguments can be further explained below:

* `associationName` : This is the name of the property on the target entity that is the association
* `alias` : This is the alias to assign it so you can reference it later in the criterions following it
* `joinType` : By default it is an inner join.  The available joins are: `INNER_JOIN, FULL_JOIN, LEFT_JOIN`
* `withClause` : This is the criterion \(so it's a restriction\) to be added to the join condition, basically the `ON` clause.

```javascript
// Using virtual services
function findAllAdmins(){
     return newCriteria()
          .isTrue( "active" )
          .joinTo( "role", "r" )
               .eq( "r.slug", "admin" )
          .list();
}

// no join type, but withClause
r = newCriteria()
     .joinTo(
          associationName="users",
          alias="u",
          withClause=getRestrictions().like( "u.lastName", "M%" )
     )
     .list();
```

## Inner Criterias

The last journey to query on associations is to pivot the root entity of the criteria to an association. This means that you will create a new criteria object based on the previous criteria, but now the target entity is the one you assign.  PHEW! That's a mouthful.  Basically, it's a nice way to traverse into the join and stay in that entity.

This is accomplished via the `createCriteria()` method or the nice dynamic alias: `with{entity}`\(\) method.

```javascript
/**
 * Create a new Criteria, "rooted" at the associated entity and using an Inner Join
 *
 * @associationName The name of the association property to root the restrictions with
 * @alias The alias to use for this association property on restrictions
 * @joinType The hibernate join type to use, by default it uses an inner join. Available as properties: criteria.FULL_JOIN, criteria.INNER_JOIN, criteria.LEFT_JOIN
 * @withClause The criteria to use with the join
 */
any function createCriteria(
	required string associationName,
	string alias,
	numeric joinType,
	any withClause
)

// Dynamic Methods
with{AssociationName}( joinType )
```

The arguments can be further explained below:

* `associationName` : This is the name of the property on the target entity that is the association
* `alias` : This is the alias to assign it so you can reference it later in the criterions following it
* `joinType` : By default it is an inner join.  The available joins are: `INNER_JOIN, FULL_JOIN, LEFT_JOIN`
* `withClause` : This is the criterion \(so it's a restriction\) to be added to the join condition, basically the `ON` clause.

{% hint style="danger" %}
Now remember that you are rooting the criteria in this association, so you can't go back to the original entity properties.
{% endhint %}

```javascript
var c = newCriteria("User");
var users = c.like("name","lui%")
     .withAdmins().like("name","fra%")
     .list();
```

