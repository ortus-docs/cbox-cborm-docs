# Constructor Properties

If you override the constructor in your ORM entity, then make sure that you call the super.init\(\) with the optional arguments below:

|  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| queryCacheRegion | string | false | _\#entityName\#.activeEntityCache_ | The name of the secondary cache region to use when doing queries via this class |
| useQueryCaching | boolean | false | false | The bit that tells the class to enable query caching, disabled by default |
| useTransactions | boolean | false | true | The bit that enables automatic hibernate transactions on all save, saveAll, update, delete methods |
| eventHandling | boolean | false | true | The bit that enables event handling via the ORM Event handler such as interceptions when new entities get created, saved, enabled by default. |
| defaultAsQuery | boolean | false | true | The bit that determines the default return value for list\(\), createCriteriaQuery\(\) and executeQuery\(\) as query or array |

```javascript
component persistent="true" table="your_table" extends="coldbox.system.orm.hibernate.ActiveEntity"{

    function init(){

        setCreatedDate( now() );

        super.init(useQueryCaching=true);

        return this;
    }

}
```

