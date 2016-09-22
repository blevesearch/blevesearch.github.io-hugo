+++
author = ["Marty Schoch"]
date = "2016-09-21T10:25:38-04:00"
title = "Using blevex packages"
[menu.docs]
weight = 2
parent = 'devguide'

+++

The [blevex](https://github.com/blevesearch/blevex) repository contains optional add-ons to bleve which cannot be a part of the main repository for the following reasons:

- C dependency
- Does not fully satisfy interface contracts
- Experimental in nature

### Using blevex packages

Bleve does not directly import any of the sub-packages with the blevex repository.  If your application would like to use one of these packages, they can either import the package directly, or import the bleve config package, which then adds the ability to include blevex packages through common build-tags.

For example, if your application adds:

```go
import(
  _ "github.com/blevesearch/bleve/config"
)
```

Then when you build your application, it will automatically recognize a common set of build tags, which currently includes:

- cellar
- cld2
- cznicb
- forestdb
- icu
- kagome
- leveldb
- libstemmer
- rocksdb

### KV Stores

#### cellar

[Cellar](https://github.com/couchbaselabs/cellar) is an experimental KVStore implementation.

#### cznicb

Bleve supports using [cznic/b](https://github.com/cznic/b) as a KVStore implementation.  Although this implementation is pure Go, it does not offer reader isolation, so it does not fully satisfy the bleve interface requirements.  Under certain restrictions it can still be used safely.

#### forestdb

Bleve supports using [ForestDB](https://github.com/couchbase/forestdb) as a KVStore implementation.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).

#### leveldb

Bleve supports using [LevelDB](https://code.google.com/p/leveldb/) as a KVStore implementation.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).

#### rocksdb

Bleve supports using [RocksDB](https://github.com/facebook/rocksdb) as a KVStore implementation.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).

### Tokenizers

#### ICU

Bleve supports using the [ICU](http://site.icu-project.org/) project to tokenize words.  For most languages Bleve's built-in unicode segmentation will work fine.  However, languages which require dictionary-based segmentation may still require this library.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).

NOTE: It is recommended to get the most recent version of ICU possible.  Older versions often included with distributions do not support language based tokenization.

### Token Filters

#### detect_lang

Bleve supports using the [cld2](https://code.google.com/p/cld2/) library to determine the language of a sample of text.  This is implemented as a token filter which will return the result as a ISO 639 language code.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).

#### stemmer_filter

Bleve supports using the [libstemmer](http://snowball.tartarus.org/download.php) library to stem words for many languages.  Many of the language specific analyzers depend on this library.  This requires a C/C++ dependency, please see [Building]({{< relref "docs/Building.md" >}}).
