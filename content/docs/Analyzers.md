+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Analyzers"
[menu.docs]
weight = 1
parent = 'analysis'

+++

## General Purpose Analyzers

### Keyword

The Keyword Analyzer does not perform any analysis on the input text.  It creates a single token representing the entire input.  This is useful for fields where you only want exact matches.  For example, a field containing keywords or tags that may contain spaces that another analyzer might interpret as token boundaries.

### Simple

The simple analyzer performs only minimal analysis on the input.

* Tokenizer - [Letter]({{< relref "docs/Tokenizers.md#letter" >}})
* Token Filters
  * [Lowercase]({{< relref "docs/Token-Filters.md#lowercase" >}})


### Standard

The Standard Analyzer is like the Simple Analyzer but also adds English stop word removal.

* Tokenizer - [Unicode]({{< relref "docs/Tokenizers.md#unicode" >}})
* Token Filters
  * [Lowercase]({{< relref "docs/Token-Filters.md#lowercase" >}})
  * English [Stop Token]({{< relref "docs/Token-Filters.md#stop-token" >}})

### Detect Language

The Detect Language Analyzer is used to examine input text, use heuristics to determine a best guess at the language, and index the ISO 639 Language Code.

* Tokenizer - [Single]({{< relref "docs/Tokenizers.md#single-token" >}})
* Token Filters
  * [Lowercase]({{< relref "docs/Token-Filters.md#lowercase" >}})
  * [CLD2]({{< relref "docs/Token-Filters.md#cld2" >}})

## Language Specific Analyzers

* Danish
* Dutch
* English
* Finnish
* French
* Hungarian
* Italian
* German
* Norwegian
* Persian
* Portuguese
* Romanian
* Russian
* Sorani
* Spanish
* Swedish
* Thai
* Turkish
