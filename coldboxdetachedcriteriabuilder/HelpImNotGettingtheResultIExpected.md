#Help! I'm Not Getting the Result I expected!
Since we’re not writing SQL, it can sometimes be frustrating to uncover why results from Criteria Builder and Detached Criteria Builder don’t match up with what you’re expecting.

An easy way to debug in these scenarios is to enable SQL logging and actually look at the query which is ultimately executed after Hibernate does it magic.

For a good guide on setting up SQL logging for ColdFusion ORM, check out http://www.rupeshk.org/blog/index.php/2009/07/coldfusion-orm-how-to-log-sql/

Once SQL logging is setup, you’ll be able to instantly see the SQL which is executed to deliver the result you’re getting from your criteria queries. You can then run these queries independently (such as in SQL Server Management Studio), identify where the issues are, and tweak your criteria query until it’s perfect.