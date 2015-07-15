+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Ignore/Disable Sections of Documents"
[menu.docs]
weight = 16
parent = 'howto'

+++

Sometimes documents contains sections of content you simply want to ignore.  Let's imagine we have the following structure:

```go
type Person struct {
    Name string
    Addr Address
}
```

And, we've decided we don't want to index or store any of the address data.  We can accomplish this by using the following DocumentMapping.

```go
addressMapping := bleve.NewDocumentDisabledMapping()
personMapping := bleve.NewDocumentMapping()
personMapping.AddSubDocumentMapping("Addr", addressMapping)
```