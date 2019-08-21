# Criteria Builder

The ColdBox Hibernate Criteria Builder is a powerful object that will help you build and execute [hibernate criteria queries](http://docs.jboss.org/hibernate/core/3.3/reference/en/html/querycriteria.html) in a fluent and dynamic manner. [HQL](http://docs.jboss.org/hibernate/core/3.6/reference/en-US/html/queryhql.html) is extremely powerful, but some developers prefer to build queries dynamically using an object-oriented API, rather than building query strings and concatenating them in strings or buffers. 

The ColdBox Criteria builder offers a powerful programmatic DSL builder for Hibernate Criteria queries. You can see below some of the Hibernate documentation on criteria queries.

1. Hibernate Criteria Queries: [http://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html](http://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html)
2. Hibernate Criteria Interface: [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/Criteria.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/Criteria.html)
3. Hibernate Restrictions: [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html)

As you will soon discover, they are fantastic but doing it the java way is not that fun, so we took our lovely ColdFusion dynamic language funkyness and added some ColdBox magic to it.

{% hint style="info" %}
The best place to see all of the functionality of the Criteria Builder is to check out the latest [API Docs](https://apidocs.ortussolutions.com/#/coldbox-modules/cborm/).
{% endhint %}

```javascript

userService
    .newCriteria()
    .eq( "name", "luis" )
    .isTrue( "isActive" )
    .getOrFail();

userService
    .newCriteria()
    .isTrue( "isActive" )
    .joinTo( "role" )
        .eq( "name", "admin" )
    .asStream()
    .list();

userService
    .newCriteria()
    .withProjections( property="id,fname:firstName,lname:lastName,age" )
    .isTrue( "isActive" )
    .joinTo( "role" )
        .eq( "name", "admin" )
    .asStruct()
    .list();
```

