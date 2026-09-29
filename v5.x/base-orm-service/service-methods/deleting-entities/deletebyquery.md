# deleteByQuery

Delete entities by using an HQL query. Do not add the `delete` keyword to your query, it is added automatically for you, so you pass the `from ... where ...` part. The query runs as a single DML HQL `delete` statement and returns the number of records deleted.

{% hint style="danger" %}
No cascading will be done and no ORM events are fired, since the entities are not loaded into the session.
{% endhint %}

## Returns

* This function returns _numeric_ (the number of records deleted)

## Arguments

| Key           | Type    | Required | Default            | Description                                                                 |
| ------------- | ------- | -------- | ------------------ | --------------------------------------------------------------------------- |
| query         | string  | Yes      | ---                | The HQL query without the `delete` keyword                                  |
| params        | any     | No       | `{}`               | Named (struct) or positional (array) parameters                             |
| flush         | boolean | No       | false              | Flush the session after deleting                                            |
| transactional | boolean | No       | From Property      | Wrap the call in a BoxLang `transaction{}` (joins an active one if present) |
| datasource    | string  | No       | Service datasource | The datasource to use                                                       |

## Examples

```javascript
// delete all blog posts
ormService.deleteByQuery( "from Post" );

// delete query with positional parameters
ormService.deleteByQuery( "from Post as b where b.author = ?1 and b.isActive = ?2", [ "Luis Majano", false ] );

// examples with named parameters
ormService.deleteByQuery( "from Post as p where p.author = :author", { author : "Luis Majano" } );

// delete and flush
ormService.deleteByQuery(
    query  = "from User as u where u.isActive = :active",
    params = { active : false },
    flush  = true
);
```
