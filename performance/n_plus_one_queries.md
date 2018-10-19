# The 1+N queries problem

ORMs tend to be "lazy". By default they try to restrict themselves to querying the minimal amount of information necessary to get the data you ask them.

This is usually what oyu want to do, but in many situations, this can cause performance problems.

Say you are rendering a web page which lists a number of objects and prints some information about them.

For instance, assume the following schema:

```
CREATE TABLE students (
    student_id               serial primary key,
    name                     text not null,
    ...
);
```

And you want to list all students, showing their names. If you ask an ORM to get a list of all students, it might execute a query such as:

```
SELECT student_id FROM students
```

to get the list, creating a stub object which forces the following query to be issued when trying to access its name property:

```
SELECT name[,...] FROM students WHERE student_id = ?
```

If the ORM behaves this way, rendering a page containing 100 students will perform a select query to fetch the list and then one hundred queries to get each student's name.

In most situations (esp. when you are not using an in-process embedded database), each query you issue has a minimum latency cost which under normal circumstances can be largely ignored. However, if an operation issues a large number of queries, such as in this case, this can quickly add up.

Following our example, if each query has a latency of 150ms, the latency for the additional queries will add up to 15s.

This is often unsatisfactory, so many ORMs allow you to tune specific operations to eagerly load more data than what they do by default. This should allow us to force the ORM to issue the following query:

```
SELECT name[,...] FROM students
```

, fetching all the student names in one go so a single query is all needed to render the page, reducing the total page render time dramatically.

Some ORMs might eagerly load by default all fields of a table when querying for a list of items, because in most cases this is the right choice (except when some column stores a large amount of information). However, it is also common for us to fetch a list of items and then access data which is not directly on the tables required to list the items. While some ORMs will fetch all information in tables they are already querying, most sane ORMs will not access tables which are not strictly required to perform the actions they are asked to do.

However, most ORMs will also allow you to tweak this behavior so it prefetches information from related tables in a single query. This is something which is more appropriate to tune per-query, as it is unusual for such optimizations to be needed every time an ORM object is accessed, and if it not needed, it can have a significant negative impact.

