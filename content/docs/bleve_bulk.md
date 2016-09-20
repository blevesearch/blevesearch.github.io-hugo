+++
title = "bleve bulk"

+++

bulk loads from newline delimited JSON files

### Synopsis


The bulk command will perform batch loading of documents in one or more newline delimited JSON files.

```
bleve bulk [index path] [data paths ...]
```

### Options

```
  -b, --batch int   Batch size for loading, default 1000. (default 1000)
  -j, --json        Parse the contents as JSON, defaults true. (default true)
```

### SEE ALSO
* [bleve]({{< relref "docs/bleve.md" >}})	 - command-line tool to interact with a bleve index
