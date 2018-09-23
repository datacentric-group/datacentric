# BLOB or not

Often we need to store binary data in our application. Databases tend to have `BLOB` datatypes which are designed to store binary data, and we always have the option of storing this binary data as files in a traditional filesystem and store the path in the database.

This is a very common decision which we might come across in a multitude of situations, so it is not surprising that there is not a clear-cut decision. There are pros and cons to each option, and each must be weighed in your particular scenario; and the cons of the option you take must be then taken into consideration.

## Pros and cons of each approach

### Integrity

Data-integrity is one of the biggest reasons to use a relational database, and `BLOB`s score high here. Storing files in the filesystem and keeping a path reference in the database is prone to problems:

* Moving or removing a file referenced in the database without updating the reference will leave a dangling link and thus integrity will be broken
* Conversely, if you update a path in the database (say because a database entity must reference a different blob), the old file can be left dangling with no references

As we are not aware of a system which allows us to enforce transactions across a database or filesystem, these problems can be mitigated but cannot be completely prevented easily.

#### Backups

Taking consistent backups between a database and a filesystem is also not easily achievable, esp. if they reside in different systems. The more frequently this kind of information is updated, the more likely a backup will be inconsistent.

### Performance

The other side of the coin is performance. It is very likely that the binary data you want to store in the database will be much bigger than the rest of the data. A single photographs, video or audio tends to be megabyte-sized, while typical scalars in the database are often a couple of bytes or in the case of text, maybe hundreds or thousands of bytes in the most common case.

This means that the size of your database will grow if you store binary data in your database, making performance more significant. Backups and other operations in the database will cost more.

Typically, systems are better-optimized to deal with files in a filesystem rather than in a database. Web servers for instance often can serve files from the filesystem using very efficient mechanisms which are not available for files stored in databases, for instance.

### Simplicity

Some things will be simpler if you store files in the filesystem, and some when they are in a database. If you already need database backup, if you store files in the filesystem you will need an additional backup mechanism to manage (additionally to the consistency problems mentioned above). If you need to export the files somewhere, database tooling is not geared to export binaries easily- while if you store the files in a convenient manner, probably exporting them will be easy.

Storing files in the filesystem is not straightforward, either- not just being careful to achieve consistency, but also problems can crop up for instance if you need to store a high number of files, as a naïve approach which stores all files in a folder might face problems due to having a large amount of files in a single directory. Strategies such as splitting files across several directories have their own associated complexity.

## Choosing

Considering all mentioned aspects, then you need to decide. One size does not fit all and different data within the same application might need different approaches. All the previous factors are affected by intrinsics of your data such as:

* Number of files
* Size of files
* Frequency of updates
* Consistency requirement
* Access/export requirements

## Implementation

Once decided, implementation tends to be straightforward for database-stored files (although some databases implementation of database-stored binaries might require additional complexity, not be well-supported in an ORM, require application code care, etc.), but for filesystem-stored files additional care might need to be taken:

* A strategy for storing files which prevents too many files stored in a single directory. Hasn-based schemes are popular for this.
* Processes to mitigate consistency issues, such as periodic scans for broken references, etc.
