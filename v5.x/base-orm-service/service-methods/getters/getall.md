# getAll

Retrieve all the instances from the passed in entity name using the id argument if specified. The id can be a single ID, a list of IDs or an array of IDs, or none to retrieve all. IDs that are not found are simply not part of the results, and the results come back in the `sortOrder` you pass, not in the order of the IDs.

You can use the `readOnly` argument to give you the entities as read only entities.

You can use the `properties` argument so this method can return to you array of structs instead of array of objects. The property list must include the `as` alias if not you will get positional keys.

Example Positional: `properties="catID,category` Example Aliases: `properties="catID as id, category as category, role as role"`

You can use the `asStream` boolean argument to get either an array of objects or a [cbStreams](https://forgebox.io/view/cbstreams) stream.

{% hint style="success" %}
No casting is necessary on the Id value type: `bx-orm` converts it to the entity's id type for you.
{% endhint %}

## Returns

* This function returns _array_ of entities found (or an array of structs when using `properties`)
* This function returns a [cbStreams](https://forgebox.io/view/cbstreams) stream if **asStream = true**

## Arguments

| Key        | Type    | Required | Default | Description                                                                                                                                                   |
| ---------- | ------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| entityName | string  | true     | ---     |                                                                                                                                                               |
| id         | any     | false    | ---     |                                                                                                                                                               |
| sortOrder  | string  | false    | ---     | The sort ordering of the array                                                                                                                                |
| readOnly   | boolean | false    | false   |                                                                                                                                                               |
| properties | string  | false    |         | If passed, you can retrieve an array of properties of the entity instead of the entire entity. Make sure you add aliases to the properties: Ex: 'catId as id' |
| asStream   | boolean | false    | false   | Returns the result as a [cbStreams](https://forgebox.io/view/cbstreams) stream                                                                                |

## Examples

```javascript
// Get all user entities
users = ORMService.getAll(entityName="User", sortOrder="email desc");
// Get all the following users by id's
users = ORMService.getAll("User","1,2,3");
// Get all the following users by id's as array
users = ORMService.getAll("User",[1,2,3,4,5]);
// Get an array of structs instead of entities
cats = ORMService.getAll( entityName="Category", properties="catID as id, category as category" );
// Get a cbStreams stream
stream = ORMService.getAll( entityName="User", asStream=true );
```
