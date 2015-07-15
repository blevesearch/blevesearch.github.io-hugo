+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Highlight Matches in Results"
[menu.docs]
weight = 26
parent = 'howto'

+++

One popular feature of search is the ability to highlight matching sections of a document in the search results.

**Requirements**
* The field you want to highlight matches in must be both **Stored** and **IncludeTermVectors**.  It must be stored so that we have access to the original content to highlight, and it must include term vectors so that we know the byte offsets where each term occurred within the original contnent.

Consider we already have the following search request:

```go
    query := bleve.NewQueryStringQuery(...)
    searchRequest := bleve.NewSearchRequest(query)
```

We can highlight results with the default highlighter using:

```go
    searchRequest.Highlight = bleve.NewHighlight()
```

Or we can request use of a specific named highlighter with:

```go
    searchRequest.Highlight = bleve.NewHighlightWithStyle(highlighterName)
```

Without any other configuration, it will automatically try to highlight any fields which have matches.  Sometimes we're only interested in highlighting a specific field or set of fields.

```go
    searchRequest.Highlight.AddField(field)
```

### Highlighters

#### HTML

The HTML Result Match Highlighter is registered with the name `html`.

By default it produces output that looks like:

![](/img/docs/highlight-html.png)

#### ANSI

The ANSI Result Match Highlighter is registered with the name `ansi`.

By default it produces output that looks like:

![](/img/docs/highlight-ansi.png)