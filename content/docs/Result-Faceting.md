+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Include Facets in Results"
[menu.docs]
weight = 36
parent = 'howto'

+++

Facets allow you to include aggregated information about the documents matching your query.

### Terms Facet

In the beer-sample dataset each beer is assigned a style.  This would be a common field to produce a facet on since it has low cardinality and can be helpful in refining your search.  To start, each facet requires a name and a size.

```go
    stylesFacet := bleve.NewFacetRequest("style", 3)
    searchRequest.AddFacet("styles", stylesFacet)
```

Here we have named the facet `styles` and requested the result to include the top 3 categories or buckets in the facet.  We have set the field to `style` which corresponds to the name we used when mapping the data.

Without providing any other configuration options this will produce what is known as a Terms Facet.  That means each term indexed for this field is treated as a unique bucket.  For each result document, we include the document in the appropriate bucket, as determined by the configured field.  (For the results to make the most sense, this type of facet is normally used for fields indexed as a single term)

### Numeric Range Facet

In the beer-sample dataset each beer has a numeric `abv` or alcohol by volume field.  With numeric range facets we can defined buckets to be bounded by numeric ranges.  In this example we define 3 buckets which cover all numeric values, corresponding to low, medium, and high alcohol content.

```go
    var lowToMidAbv = 3.5
    var midToHighAbv = 9.0
    abvFacet := bleve.NewFacetRequest("abv", 3)
    abvFacet.AddNumericRange("low", nil, &lowToMidAbv)
    abvFacet.AddNumericRange("medium", &lowToMidAbv, &midToHighAbv)
    abvFacet.AddNumericRange("high", &midToHighAbv, nil)
    searchRequest.AddFacet("abv", abvFacet)
```

### DateTime Range Facet

In the beer-sample dataset each beer and brewery has a date `updated` describing when the document was updated.  With date range facets we can defined buckets to be bounded by date ranges.  In this example we define 2 buckets which cover all date values, corresponding to those updated this year, and those updated prior to that.

```go
    var cutOffDate = time.Now().Add(-365*24*time.Hour)
    updatedFacet := bleve.NewFacetRequest("updated", 2)
    updatedFacet.AddDateTimeRange("old", time.Unix(0, 0), cutOffDate)
    updatedFacet.AddDateTimeRange("new", cutOffDate, time.Unix(999999999, 999999999))
    searchRequest.AddFacet("updated", updatedFacet)
```

### Facet Results

For each facet you build, a FacetResult is returned containing the following:

* Field - the name of the field the facet was built on
* Total - the total number of values encountered (if each document had one term, this should match the total number of documents in the search result)
* Missing - the number of documents which do not have any value for this field
* Other - the number of documents for which a value exists, but it was not in the top N number of facet buckets requested
* Array of Facets - each Facet contains the count indicating the number of items in this facet range/bucket
    * Term - Terms Facets include the name of the term
    * Numeric Range - Numeric Range Facets include the range for this bucket
    * DateTime Range - DateTime Range Facets include the datetime range for this bucket