---
description: "Concrete Virtual Services - Building custom services that inherit from Virtual Entity Services"
icon: square-code
---

# Concrete Virtual Services

![](https://raw.githubusercontent.com/wiki/coldbox-modules/cbox-cborm/ConcreteORMServices.jpg)

Let's say you are using the virtual service but you find that they do not complete your requirements, or you need some custom methods or change functionality. Then you will be building concrete services that inherit from the virtual entity service. This is the very purpose of these support classes as most of the time you will have custom requirements and your own style of coding. You will do this in two steps:

1. Inherit from `cborm.models.VirtualEntityService`
2. Call the `super.init()` constructor with the entity to root the service and any other options

Below is a sample service layer:

```javascript
class extends="cborm.models.VirtualEntityService" singleton {

    // DI
    property name="settingService"  inject="id:settingService@cb";
    property name="CBHelper"        inject="id:CBHelper@cb";
    property name="log"             inject="logbox:logger:{this}";

    /**
    * Constructor
    */
    CommentService function init(){
        super.init( entityName="cbComment", useQueryCaching="true");
        return this;
    }

    /**
    * Get the total number of approved comments in the system
    */
    numeric function getApprovedCommentCount(){
        return countWhere( isApproved = true );
    }

    /**
    * Get the total number of unapproved comments in the system
    */
    numeric function getUnApprovedCommentCount(){
        return countWhere( isApproved = false );
    }

    /**
    * Comment listing for UI of approved comments, returns struct of results=[comments,count]
    * @contentID The content ID to filter on
    * @max The maximum number of records to return, 0 means all
    * @offset The offset in the paging, 0 means 0
    * @sortOrder Sort the comments asc or desc, by default it is desc
    */
    function findApprovedComments(
        contentID,
        max=0,
        offset=0,
        sortOrder="desc"
    ){
        var results = {};
        var c = newCriteria();

        // only approved comments
        c.isTrue("isApproved");

        // By Content? Dotted paths join the association for you, and no casting is needed
        if( structKeyExists( arguments,"contentID" ) AND len( arguments.contentID ) ){
            c.isEq( "relatedContent.contentID", arguments.contentID );
        }

        // run criteria query and projections count
        results.count    = c.count();
        results.comments = c.list(
            offset    = arguments.offset,
            max       = arguments.max,
            sortOrder = "createdDate #arguments.sortOrder#",
            asQuery   = false
        );

        return results;
    }
}
```

{% hint style="info" %}
`newCriteria()` returns a `bx-orm` `entityCriteria()` builder. Check out the [Criteria Builder](../criteria-queries/criteria-builder/README.md) section for all its methods.
{% endhint %}
