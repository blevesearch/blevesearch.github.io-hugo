+++
title = "bleve query"

+++

queries the index

### Synopsis


The query command will execute a query against the index.

```
bleve query [index path] [query]
```

### Options

```
  -x, --explain        Explain the result scoring, default false.
  -f, --field string   Restrict query to field, by default no restriction, not applicable to query_string queries.
      --fields         Load stored fields, default false.
      --highlight      Highlight matching text in results, default true. (default true)
  -l, --limit int      Limit number of results returned, default 10. (default 10)
  -r, --repeat int     Repeat the query this many times, default 1. (default 1)
  -s, --skip int       Skip the first N results, default 0.
  -t, --type string    Type of query to run, defaults to 'query_string' (default "query_string")
```

### SEE ALSO
* [bleve]({{< relref "docs/bleve.md" >}})	 - command-line tool to interact with a bleve index
