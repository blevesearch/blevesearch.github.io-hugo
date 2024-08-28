+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Queries"
[menu.docs]
weight = 15
parent = 'userguide'
identifier = 'queries'

+++

Queries supported by bleve. Note the example code on this page assumes you have created a bleve index with the sample data from the [beer-search](https://github.com/blevesearch/beer-search) application.

Here is a sample record that we can target with the search examples below:
```json
{
    "name": "21A IPA",
    "abv": 7.2,
    "ibu": 0.0,
    "srm": 0.0,
    "upc": 0,
    "type": "beer",
    "brewery_id": "21st_amendment_brewery_cafe",
    "updated": "2010-07-22 20:00:20",
    "description": "Deep golden color. Citrus and piney hop aromas. Assertive malt backbone supporting the overwhelming bitterness. Dry hopped in the fermenter with four types of hops giving an explosive hop aroma. Many refer to this IPA as Nectar of the Gods. Judge for yourself. Now Available in Cans!",
    "style": "American-Style India Pale Ale",
    "category": "North American Ale"
}
```

### Term

A term query is the simplest possible query.  It performs an exact match in the index for the provided term.

Most of the time users should use a Match Query instead.

```go
q := bleve.NewTermQuery("golden")
```

### Match

A match query is like a term query, but the input text is analyzed first.  An attempt is made to use the same analyzer that was used when the field was indexed.

The match query can optionally perform fuzzy matching.  If the fuzziness parameter is set to a non-zero integer the analyzed text will be matched with the specified level of fuzziness.  Also, the prefix_length parameter can be used to require that the term also have the same prefix of the specified length.

```go
q := bleve.NewMatchQuery("golden")
```

### Phrase

A phrase query searches for terms occurring in the specified position and offsets.

The phrase query is performing an exact term match for all the phrase constituents.  If you want the phrase to be analyzed, consider using the Match Phrase Query instead.


```go
phrase := []string{"deep", "golden", "color"}
q := bleve.NewPhraseQuery(phrase, "description")
```

### Match Phrase

The match phrase query is like the phrase query, but the input text is analyzed and a phrase query is built with the terms resulting from the analysis.
```go
q := bleve.NewMatchPhraseQuery("Citrus and piney hop aromas")
```

### Prefix

The prefix query finds documents containing terms that start with the provided prefix.

```go
q := bleve.NewPrefixQuery("bud")
q.SetField("brewery_id") //limit search to field
```

### Fuzzy

A fuzzy query is a term query that matches terms within a specified edit distance (Levenshtein distance).  Also, you can optionally specify that the term must have a matching prefix of the specified length.

```go
q := bleve.NewFuzzyQuery("Citrus")
q.SetFuzziness(3)
```

### Conjunction

The conjunction query is a compound query.  Result documents must satisfy all of the child queries.

```go
tq1 := bleve.NewTermQuery("golden")
tq2 := bleve.NewTermQuery("Citrus")
q := bleve.NewConjunctionQuery([]bleve.Query{tq1, tq2})
```

### Disjunction

The disjunction query is a compound query.  Result documents must satisfy a configurable `min` number of child queries.  By default this `min` is set to 1.

```go
tq1 := bleve.NewTermQuery("golden")
tq2 := bleve.NewTermQuery("Citrus")
q := bleve.NewDisjunctionQuery([]bleve.Query{tq1, tq2})
```

### Boolean

The boolean query is useful combination of conjunction and disjunction queries.  The query takes three lists of queries:

* must - result documents must satisfy all of these queries
* should - result documents should satisfy at least `minShould` of these queries
* must not - result documents must not satisfy any of these queries

The `minShould` value is configurable, defaults to 0.

```go
mq := bleve.NewMatchQuery("golden")
mq.SetField("description")

mq2 := bleve.NewMatchQuery("piney")
mq2.SetField("description")

mq3 := bleve.NewMatchQuery("citrus")
mq3.SetField("description")

mq4 := bleve.NewMatchQuery("brilliant")
mq4.SetField("description")

q := bleve.NewBooleanQuery()
q.AddMust(mq)
q.AddMust(mq2)
q.AddShould(mq3)
q.AddMustNot(mq4)
```

### Numeric Range

The numeric range query finds documents containing a numeric value in the specified field within the specified range.  You can omit one endpoint, but not both.  The `inclusiveMin` and `inclusiveMax` properties control whether or not the end points are included or excluded.

```go
min := 5.0
max := 10.0
q := bleve.NewNumericRangeQuery(&min, &max)
q.SetField("abv") // only search this field
```

### Date Range

The date range query finds documents containing a date value in the specified field within the specified range. You can omit one endpoint, but not both. The inclusiveStart and inclusiveEnd properties control whether or not the end points are included or excluded.

### Query String

The query language query allows humans to describe complex queries using a simple syntax.  There is a separate page with the [full query language specification]({{< relref "docs/Query-String-Query.md" >}})

### Match All

The match all query will match all documents in the index.

```go
q := bleve.NewMatchAllQuery()
```

### Match None

The match none query will not match any documents in the index.

```go
q := bleve.NewMatchNoneQuery()
```

### Doc ID Query

The doc ID query will match only documents that contain one of the supplied document identifiers.

## Tips
Some helpful hints for using the search query apis.

### Specify Fields
Many queries can be limited to specified fields using the `SetField()` on the query object.
```go
fq := bleve.NewMatchQuery("ipa")
fq.SetField("name") // only search the name field for the search term
```

### Configurable Parameters
Many queries can be altered with setter methods. For example the fuzzy search query support changing the fuzzyness among others.
```go
fq := bleve.NewFuzzyQuery("citus") //note typo
fq.SetFuzziness(2)
```