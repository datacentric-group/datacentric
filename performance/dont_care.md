# You probably shouldn't care

There are many myths about relational databases' performance. As in other areas, it pays to be scientific in this regard.

Optimizing for performance is probably a waste of time unless you have clear proof that it is needed.

Most SQL dialects allow you to generate quickly large amounts of test data; they usually provide functions which can be used to create `SELECT` queries which return an arbitrary number of rows without an underlying table. Then additional columns can be added to this query with hardcoded (or even dynamic) values, then the results of the `SELECT` can be `INSERT`ed into a table.

This is often very efficient, so even generating millions of rows is relatively quick.

So in most situations it's pretty reasonable to experiment to get an idea of how a given approach will behave.

Even if you find out that the simplest solution does not perform well, once you have your test database, it is also easy to try out adding indexes, etc. and verify if the required performance is achievable.

In some other cases, it is often a good idea to estimate roughly the size of your database or specific tables. You can usually give an average byte size per row and multiply it by the expected number of rows.

Comparing this number to the amount of memory you expect the database server will have is interesting metric. If the data to manage is roughly smaller than the available memory, you can expect that your biggest problems will be caused by requiring an excessive number of queries for business operations. Accessing/modifying information that fits in RAM tends to be cached and unless you have very hard speed requirements for operations, you will probably have decent performance.

Relational databases also make it relatively easy to implement performance improvements without much cost even when your system is live, unless you have very hard downtime requirements.

However, it is always a good measure to have up to date information on the performance of your database so you can detect worrysome performance trends and attack them as soon as possible, as addressing performance problems tends to require operations whose cost increases with the amount of impacted data.

Vertical scaling is another good option which is applicable in many cases. You can probably start off with a smaller database server than what is available. This will help you save costs at the beginning, when you probably have less data to deal with. Data growth often is correlated to business growth, so buying a bigger server later on might be easier to do. In any case, it pays to perform some back of the envelope estimates early in the project to have a good idea of whether vertical scaling makes sense in your situation.

(Also note that while big database servers traditionally can be very expensive- or at least expensive enough not to be able to use one in planning stages, many cloud providers allow you to allocate a server for short periods of time, such as a couple of hours, paying proportionally to this; so you might pay less than a hundredth of the total monthly cost of a beefy server.
