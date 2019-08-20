# Usage

You just use entityNew\(\) like you would with any ORM entity and then just call our cool methods:

```javascript
// get a new User entity
user = entityNew("User");
// Count all the users in the database
usersFound = user.count();

// create a new user entity and pre-populate it with data
coolUser = user.new( {firstName="Luis",lastName="Majano",Awesome=true} );
// save the user object
coolUser.save();

// Retrieve a user with an ID of 123
luis = user.get("123");
```

