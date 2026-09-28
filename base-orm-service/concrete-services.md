---
description: "Concrete Services - Creating custom services that extend the base ORM service in CBORM."
icon: puzzle-piece
---

# Concrete Services

![](https://github.com/ColdBox/cbox-cborm/wiki/ConcreteORMServices.jpg)

Let's say you are using the virtual services and base ORM service but you find that they do not complete your requirements, or you need some custom methods or change functionality. Then you will be building concrete services that inherit from the base or virtual entity services. This is the very purpose of these support classes as most of the time you will have custom requirements and your own style of coding.

Here is a custom `AuthorService` we created:

{% code title="models/AuthorService.bx" %}
```javascript
/**
 * Service to handle author operations.
 */
class extends="cborm.models.VirtualEntityService" accessors="true" singleton {

    // User hashing type
    property name="hashType";

    AuthorService function init(){
        // init it via virtual service layer
        super.init( entityName = "bbAuthor", useQueryCaching = true );
        setHashType( "SHA-256" );

        return this;
    }

    function search( criteria ){
        var params = { criteria : "%#arguments.criteria#%" };
        return executeQuery(
            query   = "from bbAuthor where firstName like :criteria OR lastName like :criteria OR email like :criteria",
            params  = params,
            asQuery = false
        );
    }

    function saveAuthor( author, passwordChange = false ){
        // hash password if new author
        if ( isNull( getKeyValue( arguments.author ) ) OR arguments.passwordChange ) {
            arguments.author.setPassword( hash( arguments.author.getPassword(), getHashType() ) );
        }
        // save the author
        return save( arguments.author );
    }

    boolean function usernameFound( required username ){
        return ( countWhere( username = arguments.username ) GT 0 );
    }

}
```
{% endcode %}

Then you can just inject your concrete service in your handlers, or other models like any other normal model object.

```javascript
class {
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
            prc.authors        = authorService.list( sortOrder="lastName desc", asQuery=false );
            prc.authorCount     = authorService.count();
        }

        // View
        event.setView("authors/index");
    }
}
```
