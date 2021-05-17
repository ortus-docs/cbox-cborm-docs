# Criteria Builder

Hibernate provides several ways to retrieve data from the database.  We have seen the normal entity loading operations in our basic CRUD and we have seen several HQL and SQL query methods as well.  The last one is the [Hibernate Criteria Queries](https://howtodoinjava.com/hibernate/hibernate-criteria-queries-tutorial/).

The ColdBox Hibernate Criteria Builder is a powerful object that will help you build and execute [hibernate criteria queries](http://docs.jboss.org/hibernate/core/3.3/reference/en/html/querycriteria.html) in a fluent and dynamic manner. [HQL](http://docs.jboss.org/hibernate/core/3.6/reference/en-US/html/queryhql.html) is extremely powerful, but some developers prefer to build queries dynamically using an object-oriented API, rather than building query strings and concatenating them in strings or buffers. This is error prone, syntax crazy and sometimes untestable.

![](../../.gitbook/assets/image.png)

The **ColdBox Criteria Builders** offers a powerful programmatic DSL builder for Hibernate Criteria queries. It focuses on a criteria object that you will build up to represent the query to execute.  The cool thing is that you can even retrieve the exact HQL or even SQL the criteria query will be executing.  You can get the explain plans, provide query hints and much more.  In our experience, criteria queries will make your life much easier when doing complicated queries.

{% hint style="success" %}
**Tip**: You don't have to use the ORM for everything.  Please be pragmatic.  If you can't figure it out in 10 minutes or less, move to direct SQL.
{% endhint %}

As you will soon discover, they are fantastic but doing it the Java way is not that fun, so we took our lovely ColdFusion dynamic language funkyness and added some ColdBox magic to it.

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

## Resources

You can see below some of the Hibernate documentation on criteria queries.

1. [http://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html](http://docs.jboss.org/hibernate/core/3.5/reference/en-US/html/querycriteria.html)
2. [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/Criteria.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/Criteria.html)
3. [http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html](http://docs.jboss.org/hibernate/core/3.5/javadoc/org/hibernate/criterion/Restrictions.html)
4. [https://www.baeldung.com/hibernate-criteria-queries](https://www.baeldung.com/hibernate-criteria-queries)
5. [https://howtodoinjava.com/hibernate/hibernate-criteria-queries-tutorial/](https://howtodoinjava.com/hibernate/hibernate-criteria-queries-tutorial/)



