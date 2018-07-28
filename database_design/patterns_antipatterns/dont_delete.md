# Don't delete

Most applications must the "deletion" of information. This has several forms:

* Remove information which must not be stored any more. Some legislation forbids storing some data for more than a specific period of time, or if a request to delete it is made
* Drop historical information which is costly to store or slows down access of more recent information
* Remove from view information which is no longer immediately useful and clutters the application

Of those, just the first one translates clearly to a `DELETE` statement (and even then, you should consider that this information will still be in backups). It is also worth discussing how these procedures should go, esp. if there is some expectation of being able to undo this action.

It is sometimes necessary to drop historical information, though. However, we recommend not doing this gratuitously. While deleting big chunks of data can look like an obvious way to speed a database, it is not always effective. Always follow the proper optimization procedures before removing historical data. When it is inevitable (or the most effective option) to delete old data, always try to use database features such as partitioning to do that.

The last case is the most complex one. Customers become ex-customers, assets are sold, etc. and users regards them as clutter.

We believe the default option should be to register data status, such as "active" and "inactive", and hide them from lists and search by default. This way:

* The information can still be accessed, used for statistical purposes, etc.
* The status can be reverted in case "deletion" was a mistake

This can be complex because in some cases, "inactive" entities have related entities which must also be "hidden". There are two ways to consider this:

* Related entries status' should be calculated in terms of its parent entries; a customer asset can be inactive because it specifically has been made inactive or because the whole customer has been made inactive.
* Status should be propagated in cascade to children entities; when a customer is deactivated, all its assets are marked as inactive
 
While those have different implementations, costs and overheads (notably, calculating state can be complex and become a cross-cutting concern in your code), the foremost consideration is the desired behavior of the system. A calculated status means that if the parent entity is made active again, all children entities revert immediately to their pre-deactivation status. Cascading deactivations don't behave like this and it's hard to implement that behavior.

Implementing status is a big concern in any case; active/inactive status probably needs to be taken into consideration in most places in your application, and forgetting to do so can result in serious bugs. In situations where this is critical, we recommend dealing with this in the most automated way possible; whether using SQL views to create "active tables" and use them by default or using facilities in your development stack to automatically filter out inactive records by default.

Finally, it should be considered that in some specific circumstances, deletion of data can be legit; mistakes, etc. can warrant deleting data which would be even clutter in views of inactive data or which should not pollute statistics, etc.
