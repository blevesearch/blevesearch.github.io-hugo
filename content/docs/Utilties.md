+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Command-line Utilities"
[menu.docs]
weight = 20
parent = 'userguide'

+++
bleve comes with a few helpful command-line utilities:


### bleve_create

The bleve_create command allows you to create a new bleve index from the command line.

* -index - path to the index to create
* -mapping - path to a JSON serialized index mapping to be used for this index

### bleve_dump

The bleve_dump utility allows you to dump out part of all of a bleve index.

* -docID - dump all the rows pertaining to the single specified document
* -fields - dump the field names and field ids in the index
* -index - path to the index to dump
* -mapping - print a JSON serialization of the index mapping

If none of these alternate modes have been specified it will dump all rows in the index.

### bleve_index

The bleve_index utility takes one or more files or directories and adds them to the specified index.

Document IDs are generated based on the filename.  By default the direction portion and extension are stripped off.  For example, indexing `../data/file.json` would index the contents with the document ID `file`.

* -index - the path to the index
* -keepDir - keep the directory portion of the file name
* -keepExt - keep the extension portion of the file name

### bleve_query

The bleve_query command allows you to run queries on a bleve index.

* -explain - include scoring explanation in the output
* -highlight - highlight matching portions of results
* -index - path to the index to query
* -limit - limit the results to the first `limit` number of matches
* -skip - skip the first `skip` number of matches

### bleve_registry

The bleve_registry command lets you inspect which named components were compiled into this version of bleve.  As bleve allows for optionally including or excluding a number of components, this can be a helpful way to see what you have available.