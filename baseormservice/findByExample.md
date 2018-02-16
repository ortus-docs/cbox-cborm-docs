Find all/single entities by example


###Returns

* This function returns *array*


###Arguments

| Key | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| example | any | Yes | --- | The entity sample |
| unique | boolean | false | false | Return an array of sample data or none |

###Examples

```javascript
currentUser = ormService.findByExample( session.user, true );
```

