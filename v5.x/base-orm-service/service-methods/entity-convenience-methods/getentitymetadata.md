# getEntityMetadata

This method will return to you Hibernate's metadata for a specific entity: its Hibernate 7 entity persister.

{% hint style="warning" %}
**Changed in cborm 6**: this method used to return a Hibernate 5 `ClassMetadata` object. It now returns the Hibernate 7 `EntityPersister`, which keeps the `ClassMetadata` methods you are used to: `getPropertyNames()`, `getPropertyTypes()`, `hasIdentifierProperty()`, `getIdentifierPropertyName()`, `getIdentifierType()`, etc. If you want the same information as a BoxLang struct, use the `bx-orm` `entityGetMetadata()` function instead.
{% endhint %}

## Returns

* The Hibernate Java `EntityPersister` object ([https://docs.jboss.org/hibernate/orm/7.0/javadocs/org/hibernate/persister/entity/EntityPersister.html](https://docs.jboss.org/hibernate/orm/7.0/javadocs/org/hibernate/persister/entity/EntityPersister.html))

## Arguments

| Key    | Type | Required | Default | Description                      |
| ------ | ---- | -------- | ------- | -------------------------------- |
| entity | any  | Yes      | ---     | The entity name or entity object |

## Examples

```javascript
var md = ORMService.getEntityMetadata( entity );

var idName    = md.getIdentifierPropertyName();
var propNames = md.getPropertyNames();

// A BoxLang struct of the entity metadata via bx-orm
var info = entityGetMetadata( "User" );
```
