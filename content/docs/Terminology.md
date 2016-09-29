+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Terminology"
[menu.docs]
weight = 10
parent = 'userguide'

+++

### Analyzer
An analyzer transforms input Text into a [Token Stream]({{< relref "docs/Terminology.md#token-stream" >}}).  Analyzers are composed of one or more constituent pieces to form a pipeline.  The pipe consists of zero or more [Character Filters]({{< relref "docs/Terminology.md#character-filter" >}}), followed by a single [Tokenizer]({{< relref "docs/Terminology.md#tokenizer" >}}), followed by zero or more [Token Filters]({{< relref "docs/Terminology.md#token-filter" >}}).  The input Text is run through this pipeline to produce the resulting Token Stream.

### Character Filter
A character filter processes input text to remove undesirable characters.  For example, if your input documents are HTML pages, you might use a character to remove the HTML tags.  Sometimes character filters replace the input characters with whitespace so as not to disturb the original byte offsets of the remaining text.

### Term
A term is a sequence of unicode characters.  Typically the word "term" is reserved for uses describing the things we write into indexes or the things we're looking for in indexes.  For example, the text "mary had a little lamb", might result in 3 terms being inserted into the index: "mary", "little", and "lamb".

### Token
A token is an occurrence of a [term]({{< relref "docs/Terminology.md#term" >}}) at a particular location in a document or field.

### Tokenizer
A tokenizer takes input [Text]({{< relref "docs/Terminology.md#text" >}}) and splits it into one or more [Tokens]({{< relref "docs/Terminology.md#token" >}}).  Typically for natural language it is desirable to split on word boundaries.

### Token Filter
A token filter processes each token in a token stream and results in another token stream.  This could be the original stream unmodified, or it could add, modify and remove tokens.

### Token Stream
A token stream is a sequence of [Tokens]({{< relref "docs/Terminology.md#token" >}}).

### Text
Text is a general term for a sequence of unicode characters.  Typically the word "text" is reserved for use cases where the characters have not yet been analyzed.  We start with input text, then we analyze it to produce terms to be stored in the index.
