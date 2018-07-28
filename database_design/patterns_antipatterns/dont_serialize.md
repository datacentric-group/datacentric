# Don't serialize

In many situations, you will be writing software that works with a database and you will have some data structure in the programming language you are using that you need to store in the database.

There exist many systems which can automatically "serialize" a value to a text or binary representation; many languages have such facilities built-in (such as [Java's Object Serialization](https://docs.oracle.com/javase/8/docs/technotes/guides/serialization/index.html) or [Python's pickle](https://docs.python.org/3/library/pickle.html)) or other technologies such as Protocol Buffers.

It is tempting to use them to store complex structures in the database, as it is effortless to serialize and deserialize objects and work with them.

However, data stored in this form will be opaque to the database unless explicitly supported (some databases do support XML and JSON datatypes). This means that you cannot use the information contained in this data for queries, nor it can be updated; you will need to go through your application for modifications and queries. The database won't be able to apply constraints to this data; so references can be left dangling, data can be malformed, etc.

If the storage format is also specific to some platform, this data will also be hard to work with from other platforms (say, a Java program won't be able to manipulate Python-pickled data). Given that data can sometimes outlive applications, this is a serious concern.
