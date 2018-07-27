# Primary keys

Although relational database theory establishes that all relations have a primary key, most implementations don't force you to create one.

However, only in the very extreme corner case of being forced by some unusual reason to allow the existence of duplicated rows, you should always define a primary key for every table, even if it's just a composite key of all columns.

## Primary key choice

So every time you create a table, you will have to think about its primary key. Many different strategies exist:

### Natural keys

Some information already has a "natural key"; a set of columns (which might consist of a single key) which uniquely identifies a row. If such natural key exists, it should be our first consideration to use it as the primary key.

It pays to be careful, though; in many cases such natural keys are not truly natural keys. If they come from an external source, there could exist duplicates even though we have been assured that this cannot happen. Mistakes happen and things change, so we should evaluate the risk of choosing a natural key which is not under our control.

At a later point we could also need to incorporate data from another source which does not follow the same natural key, or requirements could change, etc.

Let's consider the following example:

We are writing software to manage a gym for a university's students. University students have a unique number identifying them, so we create the following table:

```
create table users (
	student_id           integer primary key,
	name                 text not null,
[...]
```

and other tables such as:

```
create table payments (
    payment_id           integer primary key autoincrement,
	student_id           integer references users(student_id),
	when                 timestamp not null default now(),
[...]
```

However, at a later point, the gym decides to open up to people who are not students in the university. The university does not want to assign a unique code to those people, because they have no need to.

At this point, you have a problem; `student_id` is no longer a natural key. There are many ways to solve this, and given this situation you can refactor your database. However, not using `student_id` as the natural key from the beginning might have been the best option.

### Surrogate keys

A surrogate key is a unique identifier created specifically for the database, under control of the database developers.

While natural keys are somewhat convenient, especially if they are heavily used in your environment, surrogate keys tend to be more "future-proof".

#### Integer auto-incrementing surrogate keys

The simplest surrogate key is an autoincrement column. All relational databases support them and work with them efficiently.

#### UUIDs

[UUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier) are 128-bit numbers for which generation algorithms exists which virtually guarantee no collisions. As such, they are fit to be used as surrogate keys.

UUIDs are especially interesting in distributed systems, as two independent systems can generate their own UUIDs and later on their information can be combined without collisions. If your database is distributed, it is probably a good idea to use UUIDs as a surrogate key for all tables which might be created in more than a single system.

This includes cases such as providing multiple instances of your software. If you provide independent production, staging, etc. installations of your software, at some point your users might want to move data from one instance to other. Although this might not be an advisable scenario in many situations, using UUIDs can simplify shipping data from one environment to another.

UUIDs have their drawbacks, so their use should be carefully evaluated. While they can affect performance, they can be a bit unwieldy; their most common human-readable representation looks like this `bce1143e-bb15-465b-a8da-554eabdee9a0`, which is much more work to type out manually, or spell out over the phone, etc. than most typical autoincrementing keys.

### Composite primary keys

While composite primary keys could be considered strictly natural keys in many scenarios under some viewpoints, we prefer to discuss them separately from single-column natural keys.

For many tables, especially N-M relationship tables, etc. you can find a combination of columns which must not allow duplicates, such as combinations of the primary keys from the tables they reference.

In this case, it's unnecessary to have a surrogate key. However, some systems (e.g. an ORM) might not support composite primary keys, so in some cases you will be forced to add a surrogate key anyway. Going from one situation to the other should be relatively painless, as most of the time such tables don't have other tables referencing them. In cases where other tables refer to this tables, then it's probably a good idea to add a surrogate key.
