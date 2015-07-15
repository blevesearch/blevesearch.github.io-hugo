+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Character Filters"
[menu.docs]
weight = 10
parent = 'analysis'

+++

### Regular Expression

The regular expression character filter is configured with a regular expression and a replacement array of bytes.  All sequences of characters matching the regular expression are replaced with the replacement bytes.

Typically, characters that are undesirable for indexing are replaced with whitespace.  This allows the original byte offsets in the original input to remain unaffected.

### HTML

The html character filter attempts to identify HTML tags from the input text and replace them with spaces.  The current implementation is an instance of the Regular Expression character filter.

### Zero-width Non-Joiner

The zero-width non-joiner character filter replaces zero-width non-joiner characters with a space.