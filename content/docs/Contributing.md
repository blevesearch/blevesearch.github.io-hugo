+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Contributing"
[menu.docs]
weight = 4
parent = 'devguide'

+++

As bleve is a Couchbase project we must require contributors accept the [Couchbase Contributor License Agreement](http://review.couchbase.org/static/individual_agreement.html).  To sign this agreement log into the Couchbase [code review tool](http://review.couchbase.org/).  The bleve project does not use this code review tool but it is still used to track acceptance of the contributor license agreements.

All types of contributions are welcome, but please keep the following in mind:

1.  If you're planning a large change, you should really discuss it on the [google group](https://groups.google.com/forum/#!forum/bleve) first.  This helps avoid duplicate effort and spending time on something that may not be merged.
2.  Existing tests should continue to pass, new tests for the contribution are nice to have.
3.  All code should have gone through `go fmt`
4.  All code should pass `go vet`