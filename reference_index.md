* Data first systems design
* Database design
  * Some information is not relational
  * Principles
    * CONSTRAINT all the things
    * Primary keys
    * Cardinality sins
    * Business operations should map to database operations
    * Normal forms
    * Sometimes denormalization isn't
  * Patterns and antipatterns
    * Hierarchies
    * Don't fear the JOIN
    * Don't fear the CASCADE
    * Ordered lists
    * Subtyping
    * "Extensible" schemas, JSON/XML
    * Don't serialize
    * Don't delete
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
  * You probably shouldn't care
  * The 1+N queries problem
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
  * Import/Export
  * Anonymization
  * Non-relational databases
    * Graph databases
    * Key-value stores
    * Document stores
