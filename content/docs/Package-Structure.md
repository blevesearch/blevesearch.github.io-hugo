+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Package Structure"
[menu.docs]
weight = 6
parent = 'devguide'

+++

bleve is organized into a large number of packages.

## bleve

The top-level bleve package is designed to provide an easy to use wrapper around all of the lower-level packages.

## Analysis 

The analysis package contains all of the code related to analyzed text.  Generally this package is independent of everything else.  Should not depend on the index or search packages.

#### Analyzers

The analyzers package contain pre-built analyzers to for general purpose usage.

#### ByteArrayConverters

The byte array converters package contains utilities for interpreting byte arrays in common ways.

#### CharFilters

The char filters package contains implementations of the CharFilter interface.

#### DateTime Parsers

The date time parsers package contains implementation of the DateTimeParser interface.

#### Language

The language package contains sub-packages for language specific analysis.

#### Token Filters

The token filters package contains implementation of the TokenFilter interface.

#### Token Maps

The token maps package supports maintaining word or token lists.

#### Tokenizers

The tokenizers package contains implementations of the Tokenizer interface.

## Document

The document package contains the code related to bleve documents and fields.  Documents contain fields.  This is the unit of indexing within bleve.

## Index

The index package contains all of the code related to putting bits on disk in such a way to facilitate searching later.

#### Store
The store package defines an general KV store interface.  This interface allows index implementations to plug in alternative KV stores easily.

#### upside_down
The upside_down package is the inverted index implementation.  It can use any Store implementation.  This has all of the details around how individual rows are encoded.

## HTTP

An optional set of HTTP handler which expose bleve functionality over HTTP/JSON.

## Registry

The registry package provides a convenient mechanism for applications to refer to search components through string names.  This also facilitates serializing index mappings and persisting them along with the index.

## Search

The search package contains all of the code to implement search functionality.  Depends on the interfaces exposed by the Index package, but should not depend on any of its implementation details.

#### Collectors

The collectors package is responsible for collecting the desirable results out of all the results.  Typically the top N by some criteria.

#### Facets

The facets package is responsible for building facet information from result sets.

#### Highlight

The highlight package is responsible for producing highlighted matching text in search results.

#### Scorers

The scorers package is responsible for scoring search result hits.  These results may be final or intermediate results.

#### Searchers

The searchers package contain the actual searcher implementations.

## Utils

This package contains all of the command-line utilities.