+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Text Analysis"
[menu.docs]
weight = 12
parent = 'userguide'
identifier = "analysis"

+++

**Text Analysis** is the process of transforming input text into a series of analyzed terms.  Analysis is done at index time to transform input documents into index terms.  Analysis is also done at query time to transform query input into the index terms we will search form.

### Example

Let's imagine we have a document containing the text, "watery light beer".
And let's imagine a user searches for "watered down".

There is no exact match in this case, but if we want this query to match this document we can accomplish this by choosing the right analyzer.  In this case we would want to use an analyzer that can stem English words.

By doing this, the index entry for the document will contain the terms: "water", "light", "beer".
And the query will search for terms: "water", "down".

Now we see that the terms will match and the user will get search results.

Stemming is just one way in which the text can be transformed, the key here is that choosing the right analyzer is essential to getting high quality search results.

### Text Analysis in bleve

[Analyzers]({{< relref "docs/Analyzers.md" >}}) are used to transform input text into a stream of tokens for indexing.  In bleve, Analyzers are built up from modular components.

* [Character Filters]({{< relref "docs/Character-Filters.md" >}}) strip out undesirable characters from the input.
* [Tokenizers]({{< relref "docs/Tokenizers.md" >}}) split input strings into a token stream.  Typically you're trying to create a token for each word.
* [Token Filters]({{< relref "docs/Token-Filters.md" >}}) are chained together to perform additional post processing on the token stream.
