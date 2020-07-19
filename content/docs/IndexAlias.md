+++
author = ["Marty Schoch"]
date = "2016-09-21T10:25:38-04:00"
title = "Index Aliases"
[menu.docs]
weight = 17
parent = 'userguide'

+++

Bleve has a powerful feature called an `IndexAlias`.  The IndexAlias can be used to search an index, with the additional ability to atomically switch the underlying physical index.  IndexAliases can also be used to search across multiple indexes at the same time and correctly merge the results.

### IndexAlias Interface
First, its useful to note that thanks to Go interfaces, an IndexAlias is also an Index.  This means that in general you work with an IndexAlias just as you would work with an Index.  The alias concept also adds 3 methods:  Add(), Remove(), and Swap()

Add, will add one or more indexes to the alias.
Remove will remove one or more indexes from the alias.
Swap will atomically add/remove the provided indexes.  (all other ops, like search will see the alias before or after all adds/removes are done)

### Switching Indexes with Zero Downtime

1.  Create a new Index
2.  Create an IndexAlias pointing to the Index
3.  Write your application to query the IndexAlias
4.  ...At some later time, create a new Index
5.  Call Swap() on the IndexAlias passing in the new Index

Your application will safely switch to searching the new index.

### Searching Across Multiple Indexes

1.  Create more than one Index
2.  Create the IndexAlias pointing to these indexes
3.  Write your application to invoke Search() on the IndexAlias

Your application will search across all the underlying indexes at the same time, and the results will be merged together into a single result.

#### Limitations

When and IndexAlias points to multiple indexes not all operations are supported.  Only operations which can be invoked on all the child indexes and have their responses aggregated are supported.  Operations that are not supported return `bleve.ErrorAliasMulti`.
