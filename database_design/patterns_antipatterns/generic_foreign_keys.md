# Generic foreign keys

Some ORMs and frameworks allow defining special foreign keys which reference multiple different tables. They use schemas such as:

```
CREATE TABLE users (
    id                       serial primary key,
    ...
);

CREATE TABLE posts (
    id                       serial primary key,
    ...
);

CREATE TABLE comments (
    id                       serial primary key,
    commented_object_id      integer not null,
    commented_object_type    enum(users|posts),
    ...
);
```

This sample schema would allow creating `comments` on `users` *and* `posts`.

Typically, these work more or less transparently when using your ORM, but often they have limitations.

First and foremost, referential integrity tends to be lost. Unless they spend significant effort, dangling references can be created and deletion/updates on referenced tables won't enable [cascade behaviors](dont_fear_the_cascade.md).

Similarly, making queries that join tables referenced by those kinds of keys are significantly harder, requiring contortions such as:

```
SELECT *
FROM   comments
JOIN   posts ON comments.commented_object_id = posts.id AND comments.commented_object_type = posts
JOIN   users ON comments.commented_object_id = users.id and comments.commented_object_type = users
```

We advise not using this kind of functionalities, but use approaches which fit better the relational model such as [subtyping](subtyping.md).
