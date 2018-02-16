#Concrete Services
![](https://github.com/ColdBox/cbox-cborm/wiki/ConcreteORMServices.jpg)

 Let's say you are using the virtual services and base ORM service but you find that they do not complete your requirements, or you need some custom methods or change functionality. Then you will be building concrete services that inherit from the base or virtual entity services. This is the very purpose of these support classes as most of the time you will have custom requirements and your own style of coding. Here is a custom `AuthorService` we created: 

```js

/**
* Service to handle author operations.
*/
component extends="cborm.models.VirtualEntityService" accessors="true" singleton{
	
	// User hashing type
	property name="hashType";
	
	AuthorService function init(){
		// init it via virtual service layer
		super.init(entityName="bbAuthor", useQueryCaching=true);
	    setHashType( "SHA-256" );
	    
		return this;
	}
	
	function search(criteria){
		var params = {criteria="%#arguments.criteria#%"};
		var r = executeQuery(query="from bbAuthor where firstName like :criteria OR lastName like :criteria OR email like :criteria",params=params,asQuery=false);
		return r;
	}

	function saveAuthor(author,passwordChange=false){
		// hash password if new author
		if( !arguments.author.isLoaded() OR arguments.passwordChange ){
			arguments.author.setPassword( hash(arguments.author.getPassword(), getHashType()) );
		}
		// save the author
		save( author );
	}

	boolean function usernameFound(required username){
		var args = {"username" = arguments.username};
		return ( countWhere(argumentCollection=args) GT 0 );
	}

}
```

Then you can just inject your concrete service in your handlers, or other models like any other normal model object.

```js
component{
	// Concrete ORM service layer
	property name="authorService" inject="security.AuthorService";
	// Aliased 
	property name="authorService" inject="id:AuthorService";

	function index( event, rc, prc ){
		// Get all authors or search
		if( len(event.getValue( "searchAuthor", "" )) ){
			prc.authors = authorService.search( rc.searchAuthor );
			prc.authorCount = arrayLen( prc.authors );
 		} else {
			prc.authors		= authorService.list( sortOrder="lastName desc", asQuery=false );
			prc.authorCount 	= authorService.count();
		}

		// View
		event.setView("authors/index");
	}
}
```

