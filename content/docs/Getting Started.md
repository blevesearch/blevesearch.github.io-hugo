+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Getting Started"
[menu.docs]
weight = 2
parent = 'userguide'

+++


The simplest way to get started with bleve is to use the standard go get operation:

```
go get github.com/blevesearch/bleve/...
```

This will build a pure Go version of bleve and install the command-line utility.

### Your first bleve program

Create a new package, edit main.go and paste:

```go
package main

import (
	"fmt"

	"github.com/blevesearch/bleve"
)

func main() {
	// open a new index
	mapping := bleve.NewIndexMapping()
	index, err := bleve.New("example.bleve", mapping)
	if err != nil {
		fmt.Println(err)
		return
	}

	data := struct {
		Name string
	}{
		Name: "text",
	}

	// index some data
	index.Index("id", data)

	// search for some text
	query := bleve.NewMatchQuery("text")
	search := bleve.NewSearchRequest(query)
	searchResults, err := index.Search(search)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(searchResults)
}
```

This should compile, run, and return one search hit for the item added.
