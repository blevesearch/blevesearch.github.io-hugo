+++
author = ["Marty Schoch"]
date = "2016-09-21T10:25:38-04:00"
title = "Sorting"
[menu.docs]
weight = 16
parent = 'userguide'

+++

Bleve now gives you the ability to customize the order of your search results.

The default behavior is to sort results by relevance, with the highest scoring results first.

### Simple API

The simple API adds a `SortBy()` method to the SearchRequest.  Here is an example which sorts by the field `age`.

```go
searchRequest.SortBy([]string{"age"})
```

The argument is an array of strings.  Each string refers to the name of a field.  The names of fields can be prefixed with the `-` character, which will cause that field to be reversed (descending order).  Items will first be sorted by the first field.  Any items with the same value for that field, are then also sorted by the next field, and so on.  All fields in the sort order **MUST** be indexed.

There are two special fields defined for use:

  - `_id` - refers to the document identifier
  - `_score` - refers to the relevance score computed by Bleve

Here is a more complex example:

```go
searchRequest.SortBy([]string{"age", "-_score", "_id"})
```

This will first sort results by the "age" field.  If two documents have the same value for this field, they will then be sorted by the score descending, finally, if documents have the same age and score, they will be sorted by document ID ascending.

### Advanced API

The simple API works for most cases, but there are some which require the advanced API.  In these cases you must import the bleve `search` sub-package, and build a `search.SortOrder` object.  This is a slice of `search.SearchSort` objects.  Today there are three implementations available, SortField, SortScore, and SortDocID.

By building these structures manually, you can avoid ambiguities and limitations of the simple API.

- Sort by fields which happen to start with `-`
- Sort by fields which happen to conflict with `_id` or `_score`
- Control whether documents missing a field value should sort first or last
- Control which value for multi-value fields should be used for sorting
- Force handling field values as a particular type (defaults to auto)
