Retrieve all the instances from the passed in entity name using the id argument if specified. The id can be a list of IDs or an array of IDs or none to retrieve all. If the id is not found or returns null the array position will have an empty string in it in the specified order


###Returns

* This function returns *array* of entities found


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| entityName | string | Yes | --- |  |
| id | any | No | --- |  |
| sortOrder | string | false | --- | The sort orering of the array |

###Examples

```javascript
// Get all user entities
users = ORMService.getAll(entityName="User", sortOrder="email desc");
// Get all the following users by id's
users = ORMService.getAll("User","1,2,3");
// Get all the following users by id's as array
users = ORMService.getAll("User",[1,2,3,4,5]);
```