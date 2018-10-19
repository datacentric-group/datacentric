# Subtyping

Many applications handle entities which have subtypes. For instance, a school management system might handle people which are subdivided into students and teachers. There is information common to both subtypes (such as the name) which we might want to query uniformly (e.g. list all people in the school named "John"). However, there might be information specific to each subtype (such as the department teachers belong to and students' enrollment date).

Storing this in a single table such as:

```
CREATE TABLE people (
    person_id                serial primary key,
    name                     text not null,
    department_id            integer null references deparments(department_id),
    enrollment_date          date null,
    ...
);
```

can be problematic, as maybe a teacher *must* belong to a department, but we cannot make this column non-null, as students do not belong to departments.

The natural solution is to have tables for each subtype:

```
CREATE TABLE people (
    person_id                serial primary key,
    name                     text not null,
    ...
);

CREATE TABLE teachers (
    person_id                integer primary key references people(person_id),
    department_id            integer not null references deparments(department_id),
    ...
);

CREATE TABLE students (
    person_id                integer primary key references people(person_id),
    enrollment_date          date not null,
    ...
);
```

A teacher has an entry on both the `teachers` and `people` tables. This allows to query common properties uniformly while being able to enforce strict constraints on subtypes' columns.

Of course, this might mean that queries on subtypes might require additional joins, such as if we need to query for teachers in the biology department named "John".

Note that, by default, this allows a person to be both a student and a teacher at the same time. Unless strictly required, it is a good idea to design your application so these cases are handled gracefully (even if initially you disallow them).

Also, note that foreign keys targetting subtypes should reference the most specific table. If you have an address table for both teachers and students, it should reference the `people` table. If each course has an assigned teacher, the `teachers` table should be referenced.

## Type column

If you perform an operation on a supertype, such as querying for people named "John", it will not be immediately obvious whether the returned rows are teachers or students (or both). It is not extremely complex to figure this out by querying the subtype tables, but in some situations you might want to store information in the supertype table which identifies each row's specific subtype.

However, note that if you want to allow an entity to belong to multiple subtypes, this is not straightforward, although in many cases, if you allow this to happen, it might not actually be necessary often to be able to identify the subtype when operating on the supertype table.

## Ultrageneric types

You might be tempted to design a schema in which many entities have the same parent supertype. Having an `entity` or `object` supertype can sound interesting, as you can perform very generic queries if you the supertype has interesting columns; such as querying for all "objects" named "John", or all objects created within the last week.

While "universal search" can be a desirable and practical feature and this allows you to implement such a feature easily querying a single table, note that this might add significant complexity in other places; many queries will need additional joins if the schema is designed this way; insertions will always require dealing with multiple tables, etc.

When considering this, it is interesting to note that performance concerns often are secondary to maintenance overhead. This might be alleviated by the use of views and stored procedures which can hide away some of this complexity, although those views and stored procedures have their own cost, which can be amortized if used a lot, but it is still there.
