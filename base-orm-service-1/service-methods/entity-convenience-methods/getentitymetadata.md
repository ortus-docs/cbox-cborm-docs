# getEntityMetadata

This method will return to you the hibernate's metadata for a specific entity.

## Returns

* The Hibernate Java `ClassMetadata` Object \([https://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/metadata/ClassMetadata.html](https://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/metadata/ClassMetadata.html)\)

## Arguments

| Key | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| entity | any | Yes | --- | The entity name or entity object |

## Examples

```javascript
var md = ORMService.getEntityMetadata( entity );
```

