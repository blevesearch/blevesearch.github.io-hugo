+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Tokenizers"
[menu.docs]
weight = 20
parent = 'analysis'

+++

### Single Token

The Single Token Tokenizer will return the entire input bytes as a single token.

### Letter

The letter tokenizer is a tokenizer that simply identifies tokens as sequences of Unicode runes that are part of the Letter category.

### Regular Expression

The Regular Expression Tokenizer will tokenize input using a configurable regular expression.  The regular expression should match token text.  See the Whitespace Tokenizer for an example of its usage.

### Whitespace

The Whitespace Tokenizer is tokenizer which simply identifies tokens as sequences of Unicode runes that are NOT part of the Space category.


### Unicode

The Unicode Tokenizer uses the [segment](https://github.com/blevesearch/segment) library to perform [Unicode Text Segmentation](http://www.unicode.org/reports/tr29/) on word boundaries.  It is recommended over the ICU tokenizer for all languages not requiring dictionary-based tokenization that is supported by ICU.

### ICU

The ICU tokenizer uses the ICU library to tokenize the input using [Unicode Text Segmentation](http://www.unicode.org/reports/tr29/) on word boundaries.

**NOTE:** This tokenizer requires bleve be built with the optional ICU package.  See the page on [Building]({{< relref "docs/Building.md" >}}).

### Exception

The exception tokenizer allows you to define exceptions.  Exceptions are sections of the input stream which match regular expressions.  These sections are left intact as single tokens.  Any input not matching these regular expressions is passed to the child tokenizer.
