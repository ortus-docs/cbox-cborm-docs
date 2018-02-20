#Concrete Virtual Services
![](https://raw.githubusercontent.com/wiki/coldbox-modules/cbox-cborm/ConcreteORMServices.jpg)

 Let's say you are using the virtual service but you find that they do not complete your requirements, or you need some custom methods or change functionality. Then you will be building concrete services that inherit from the virtual entity service. This is the very purpose of these support classes as most of the time you will have custom requirements and your own style of coding. Below is a sample templated service layer: 

 ```javascript
 component extends="cborm.models.VirtualEntityService" singleton{

    // DI
    property name="mailService"     inject="coldbox:plugin:MailService";
    property name="renderer"        inject="coldbox:plugin:Renderer";
    property name="settingService"  inject="id:settingService@cb";
    property name="CBHelper"        inject="id:CBHelper@cb";
    property name="log"             inject="logbox:logger:{this}";

    /**
    * Constructor
    */
    public CommentService function init(){
        super.init(entityName="cbComment",useQueryCaching="true");
        return this;
    }

    /**
    * Get the total number of approved comments in the system
    */
    numeric function getApprovedCommentCount(){
        var args = { "isApproved" = true };
        return countWhere(argumentCollection=args);
    }

    /**
    * Get the total number of unapproved comments in the system
    */
    numeric function getUnApprovedCommentCount(){
        var args = { "isApproved" = false };
        return countWhere(argumentCollection=args);
    }

    /**
    * Comment listing for UI of approved comments, returns struct of results=[comments,count]
    * @contentID.hint The content ID to filter on
    * @contentType.hint The content type discriminator to filter on
    * @max.hint The maximum number of records to return, 0 means all
    * @offset.hint The offset in the paging, 0 means 0
    * @sortOrder.hint Sort the comments asc or desc, by default it is desc
    */
    function findApprovedComments(contentID,contentType,max=0,offset=0,sortOrder="desc"){
        var results = {};
        var c = newCriteria();

        // only approved comments
        c.isTrue("isApproved");

        // By Content?
        if( structKeyExists(arguments,"contentID") AND len(arguments.contentID) ){
            c.eq("relatedContent.contentID",javaCast("int", arguments.contentID));
        }
        // By Content Type Discriminator: class is a special hibernate deal
        if( structKeyExists(arguments,"contentType") AND len(arguments.contentType) ){
            c.createCriteria("relatedContent")
                .isEq("class", arguments.contentType);
        }

        // run criteria query and projections count
        results.count    = c.count();
        results.comments = c.list(offset=arguments.offset,max=arguments.max,sortOrder="createdDate #arguments.sortOrder#",asQuery=false);

        return results;
    }
}
```

