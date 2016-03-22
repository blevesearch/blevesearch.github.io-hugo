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

### Regular Expression

The Regular Expression Tokenizer will tokenize input using a configurable regular expression.  The regular expression should match token text.  See the Whitespace Tokenizer for an example of its usage.

### Whitespace

The Whitespace Tokenizer is an instance of the Regular Expression Tokenizer.  It matches tokens using the regular expression:

    \p{Han}|\p{Hangul}|\p{Hiragana}|\p{Katakana}|[^\p{Z}\p{P}\p{C}\p{Han}\p{Hangul}\p{Hiragana}\p{Katakana}]+


### Unicode

The Unicode Tokenizer uses the [segment](https://github.com/blevesearch/segment) library to perform [Unicode Text Segmentation](http://www.unicode.org/reports/tr29/) on word boundaries.  It is recommended over the ICU tokenizer for all languages not requiring dictionary-based tokenization that is supported by ICU.

### ICU

The ICU tokenizer uses the ICU library to tokenize the input using [Unicode Text Segmentation](http://www.unicode.org/reports/tr29/) on word boundaries.

**NOTE:** This tokenizer requires bleve be built with the optional ICU package.  See the page on [Building]({{< relref "docs/Building.md" >}}).
