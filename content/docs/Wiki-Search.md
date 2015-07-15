+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Wiki Search"
[menu.docs]
weight = 1
parent = 'examples'

+++

The Wiki Search is an application which uses bleve to maintain a text index of all the content inside the bleve wiki.

## Why?

We believe in the concept of [dogfooding](http://en.wikipedia.org/wiki/Eating_your_own_dog_food).  By relying on our own technology we have incentive for it to work well.

## How does it work?

Github wikis are built from content (typically markdown) stored in a git repository.  For bleve, this is `git@github.com:blevesearch/bleve.wiki.git`.

In order to maintain an up to date index of this content we use a tool called [gitmirror](https://github.com/dustin/gitmirror).  Gitmirror is hooked up to the github hooks for wiki updates.  This allows us to maintain a local repository working copy will all the current wiki pages.

Next, we built a program [bleve-wiki-indexer](https://github.com/blevesearch/bleve-wiki-indexer).  This program monitors the specified directory for changes.  When it sees files updated or deleted, it performs the corresponding action in the index.  We also strip out some of the markdown processing text to improve search recall, and use git to extract some additional document metadata.

Finally, we expose the search via a REST API using the bleve HTTP handlers.  Then a little bit of HTML, CSS and JavaScript make it look presentable and integrate it with the main [bleve site](http://www.blevesearch.com)