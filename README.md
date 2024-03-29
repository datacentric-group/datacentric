# datacentric.group

datacentric.group is a community dedicated to data-centric software.

While a lot of software development is not data-centric, we believe that a massive amount of developer time is spent writing software for data management including CRUD.

While other topics such as test-driven development, microservices, devops and many others are constantly discussed online, we think that the volume of conversation around data-centric software development does not reflect its importance.

We believe that writing such software should be a solved problem, but it is too costly to develop properly in many cases, resulting in suboptimal user satisfaction.

## Objectives

1. To become a community for relevant discussion
1. To document best practices
1. To improve the state of the art

## The Data-Centric Index

This is an index of the topics we believe are relevant to the discussion. Feel free to send us pull requests to expand the index or fill in topics.

* [Data-first systems design](data_first_systems_design.md)
* [Why everything here seems to talk about relational databases?](why_everything_is_relational.md)
* Database design
  * Some information is not relational
  * Principles
    * CONSTRAINT all the things
    * [Primary keys](database_design/principles/primary_keys.md)
    * Cardinality sins
    * Business operations should map to database operations
    * Normal forms
    * Sometimes denormalization isn't
  * Patterns and antipatterns
    * Hierarchies
    * Don't fear the JOIN
    * [Don't fear the CASCADE](database_design/patterns_antipatterns/dont_fear_the_cascade.md)
    * Ordered lists
    * [Subtyping](database_design/patterns_antipatterns/subtyping.md)
    * [BLOB or not](database_design/patterns_antipatterns/blob_or_not.md)
    * "Extensible" schemas, JSON/XML
    * [Generic foreign keys](database_design/patterns_antipatterns/generic_foreign_keys.md)
    * [Don't serialize](database_design/patterns_antipatterns/dont_serialize.md)
    * [Don't delete](database_design/patterns_antipatterns/dont_delete.md)
    * Configuration tables
    * Lookup tables/enums
    * Denormalization
    * Authorization
    * Materialization
    * Temporal data
    * Auditing
    * Append-only design
  * Database refactoring
* Database performance
  * [You probably shouldn't care](performance/dont_care.md)
  * [The 1+N queries problem](performance/n_plus_one_queries.md)
  * Sharding
  * Historical tables
  * Stepping outside relational databases
* Tooling
  * RDBMS
    * Standalone
      * PostgreSQL
      * SQL Server
    * Embedded
      * H2
      * SQLite
  * Schema management/migration/version control
    * Sqitch
    * Migra
  * ORMs/language access
    * Java
      * JOOQ
      * Spring JDBC
    * Clojure
      * HugSQL
    * .NET
      * Linq
  * CRUD
    * Web
      * Django admin
    * Mobile apps
    * REST
    * GraphQL
    * ERPs
  * Testing tools
    * [Sample databases](tooling/testing/sample_databases.md)
    * Testcontainers
  * Import/Export
  * Anonymization
  * Non-relational databases
    * Graph databases
    * Key-value stores
    * Document stores

## Contribute

datacentric.group is currently in a community building phase. We are looking for people who share similar values to help us shape up our mission and vision. If you are interested:

* Create/browse Github Issues
* Send us Pull Requests
