# Data-first systems design

On some systems, data should be the foremost consideration, as it is its most valuable asset.

As such, the schema should be the principal design concern. You should create tables first then the code to make them work. This is fully compatible with iterative design practices; you don't need to design your whole schema upfront before writing application code.

This also applies to change, perform the necessary schema changes first and then refactor code to match.

This is intended to ensure that your schema is ideal and that you don't fall into bad schema design which would lead to poor quality.

Applying this approach is not trivial. Your development platform should be geared to handle this. In particular, merciless schema changes require discipline and tooling so that they can be performed without fear. Ideally, schema changes should result in static analysis showing you all the dependent source code broken as a result of the change, leading to the necessary changes to make it work. Automated testing is a more involved approach to this goal; a schema change should break tests and once all dependent code is fixed, tests should pass (or, as necessary, tests should be updated). Both automated testing and static analysis have their strengths and weaknesses, and ultimately probably a combination of both will be required.

This leads to thinking that schema changes should trigger the least amount possible of dependent code changes, so evolving your application is as cheap as possible. This means that code which duplicates information in your schema is undesirable. ORMs which force to rewrite your schema definition impose unnecessary overhead. Either they should work automatically from your schema definition, or they should derive the schema from their own schema definition. Unfortunately, the latter might be problematic as the ORM might now allow you to define your schema completely (e.g. some database entities such as triggers or procedures might be out of scope). We also advocate that the database should be self-standing and that it should not be required to go through any additional layer to access it, and an ORM-centric approach goes against this.

This goes further, as a significant part of application code tends to be schema-dependent. Can we reduce this code? Probably, by writing code which is as schema-agnostic as possible. CRUD, reporting, etc. is often pretty much the same regardless of schema. As such, it should be possible to write code which introspects the schema and thus is schema agnostic. Then, schema changes will be reflected automatically without changing code.

Writing such code has intrinsic costs, however given that such code would be re-usable across many applications, it probably can be cost-effective. Can we derive completely an application from an schema? Probably not but for the most trivial cases. However, we surely can reduce the amount of code needed.
