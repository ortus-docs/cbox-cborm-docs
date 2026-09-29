# save

Save an entity inside a transaction or not. You can optionally flush the session also.

When `eventHandling` is enabled, the service announces the `ORMPreSave` and `ORMPostSave` interception points around the save.

{% hint style="info" %}
Transactions ride the BoxLang `transaction{}` connection: if a transaction is already active, the save joins it and is committed or rolled back with it (together with any native `queryExecute()` calls). Otherwise, with `transactional = true`, the service opens its own `transaction{}` around the save.
{% endhint %}

## Returns

* This function returns the saved entity

## Arguments

| Key           | Type    | Required | Default | Description                                                                    |
| ------------- | ------- | -------- | ------- | ------------------------------------------------------------------------------ |
| entity        | any     | Yes      | ---     | The entity to save                                                             |
| forceInsert   | boolean | No       | false   | Insert as new record whether it already exists or not                          |
| flush         | boolean | No       | false   | Do a flush after saving the entity, false by default since we use transactions |
| transactional | boolean | No       | `useTransactions` | Wrap the save in a BoxLang `transaction{}`                           |

## Examples

```javascript
var user = ormService.new("User");
populateModel(user);
ormService.save(user);

// Save with immediate flush
var user = ormService.new( entityName="User", properties={ lastName : "Majano" } );
ormService.save( entity=user, flush=true );

// save() returns the entity so you can chain
var memento = ormService.save( user ).getMemento();
```
