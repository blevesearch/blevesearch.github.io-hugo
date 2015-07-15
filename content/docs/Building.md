+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Building"
[menu.docs]
weight = 2
parent = 'devguide'

+++

The core of bleve can be built without any C/C++ libraries.  This means you can get and use bleve with:

    go get github.com/blevesearch/bleve/...

However, some of the more advanced functionality depends on C/C++ libraries.  The code using these libraries can be enabled by specifying the correct build tag.

In general, bleve expects these libraries to be built and installed in the standard system locations.  If not, it is up to you to set the appropriate CGO_... environment variables so that the libraries and headers can be found.

## LevelDB

Bleve supports using [LevelDB](https://code.google.com/p/leveldb/) as a KVStore implementation.  At this time, this is the fastest KVStore implementation available for bleve.  So if maximum performance is your goal, consider building with support for LevelDB.

To include support for the leveldb KVStore implementation include the build tag:

```-tags leveldb```

## ICU

Bleve supports using the [ICU](http://site.icu-project.org/) project to tokenize words using their implementation of the standard for [Unicode Text Segmentation at Word Boundaries](http://www.unicode.org/reports/tr29/).  Some of the language specific analyzers depend on this tokenizer.

NOTE: It is recommended to get the most recent version of ICU possible.  Older versions often included with distributions do not support language based tokenization.

To include support for the analysis components requiring ICU, include the build tag:

```-tags icu```

## libstemmer

Bleve supports using the [libstemmer](http://snowball.tartarus.org/download.php) library to stem words for many languages.  Many of the language specific analyzers depend on this library.

To include support for the analysis components requiring libstemmer, include the build tag:

```-tags libstemmer```

## cld2

Bleve supports using the [cld2](https://code.google.com/p/cld2/) library to determine the language of a sample of text.  This is implemented as a token filter which will return the result as a ISO 639 language code.

To include support for the detect_lang token filter, include the build tag:

```-tags cld2```

## Short-cut to include all the optional dependencies

Building with multiple build tags can be cumbersome:

```-tags 'leveldb icu libstemmer cld2'```

As a shortcut you can instead use:

```-tags full```

## Platform Specific Instructions

The following sections are user-contributed instructions for satisfying all the dependencies on particular platforms.

### Ubuntu 14.04 LTS (Trusty Tahr)

```
$ sudo apt-get install libleveldb-dev libstemmer-dev libicu-dev svn build-essential
$ go get -u -v  github.com/blevesearch/bleve
$ cd $GOPATH/src/github.com/blevesearch/bleve/analysis/token_filters/cld2
$ svn co http://cld2.googlecode.com/svn/trunk cld2-read-only
$ cd cld2-read-only/internal/
$ ./compile_libs.sh
$ sudo cp *.so /usr/local/lib
$ go get -u -v -tags full github.com/blevesearch/bleve
```

### OS X (hacky, but similar to Ubuntu 14.04)
```
$ brew install leveldb icu4c svn # etc?
$ export CGO_LDFLAGS=-L/usr/local/opt/icu4c/lib
$  export CGO_CFLAGS=-I/usr/local/opt/icu4c/include
$ go get -u -v  github.com/blevesearch/bleve
$ cd $GOPATH/src/github.com/blevesearch/bleve/analysis/token_filters/cld2
$ svn co http://cld2.googlecode.com/svn/trunk cld2-read-only
$ cd cld2-read-only/internal/
# if you feel gutsy run:
$ perl -p -i -e 's/soname=/install_name,/' compile_libs.sh
# otherwise, just change soname= to install_name, in the two spots in compile_libs.sh
$ ./compile_libs.sh
$ sudo cp *.so /usr/local/lib
$ go get -u -v -tags full github.com/blevesearch/bleve
```