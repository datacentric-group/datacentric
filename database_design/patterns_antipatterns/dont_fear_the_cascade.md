# Don't fear the CASCADE

When defining a foreign key relationship, most databases offer the possibility to define [referential actions](https://en.wikipedia.org/wiki/Foreign_key#Referential_actions) which specify what happens when referenced data is modified.

These are normally:

* `CASCADE`. If referenced data is deleted or updated, referencing rows will be deleted or the reference will be updated to the new value
* `RESTRICT`/`NO ACTION`. The modification will be prevented
* `SET NULL`/`SET DEFAULT`. The reference will be modified to become `NULL`or some other value.

`CASCADE` sounds scary, and it is important to understand its behavior, as it is sometimes the best option.

Suppose we have three tables:

```
CREATE TABLE customers (
    customer_id               serial primary key,
    assigned_sales_person     integer not null references sales_people(sales_person_id),
    ...
);

CREATE TABLE sales_people (
    sales_person_id           serial primary key,
    ...
);

CREATE TABLE customer_assets (
    asset_id                  serial primary key,
    customer_id               integer not null references customers(customer_id),
    ...
);
```

We have two foreign keys here; every customer has a sales person assigned, and all assets belong to a customer.

`CASCADE` deletion would have two distinct functional behaviors here:

* Deleting a sales person would delete all associated customers
* Deleting a customer would delete all of its assets

One of them sounds good in some situations (although if these assets belong to the company, maybe it's not the right idea), but the other sounds like a recipe for disaster.

Most databases default to `RESTRICT`, which is probably the safest option, but in appropriate situations, `CASCADE` can be a time-saver.
