+++
author = ["Marty Schoch"]
date = "2021-01-13T14:32:38-04:00"
title = "Bleve v2.0.0 Released"
+++

This release is a new major version because it contains breaking changes to the package API. For complete details of the contents and reasoning behind these changes, please see: [#1495](https://github.com/blevesearch/bleve/issues/1495)

### Highlights

- Remove circular dependency between Bleve and Zap modules
- Make Scorch and Zap v15 the default index/segment type when using the New() method
- New option to disable freq/norm information for a field
- Types corrected for MatchQueryOperatorOr and MatchQueryOperatorAnd (see #1410)

### Deprecated features (may be removed in the future)

- upsidedown index format and all key/value adapters
- HTTP sub-package
- bleve command-line tool sub-commands: bulk, create, index, dump
- config sub-package



