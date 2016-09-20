+++
title = "bleve create"

+++

creates a new index

### Synopsis


The create command will create a new empty index.

```
bleve create [index path]
```

### Options

```
  -i, --index string     The bleve index type to use. (default "upside_down")
  -m, --mapping string   Path to a file containing a JSON represenation of an index mapping to use.
  -s, --store string     The bleve storage type to use. (default "boltdb")
```

### SEE ALSO
* [bleve]({{< relref "docs/bleve.md" >}})	 - command-line tool to interact with a bleve index
