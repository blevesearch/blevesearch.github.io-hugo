+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Building"
[menu.docs]
weight = 2
parent = 'devguide'

+++

## The instructions on this page are no longer applicable to bleve v2

Some of the [blevex]({{< relref "docs/Blevex.md" >}}) packages require C/C++ libraries.

In general, bleve expects these libraries to be built and installed in the standard system locations.  If not, it is up to you to set the appropriate CGO_... environment variables so that the libraries and headers can be found.

## Platform Specific Instructions

The following sections are user-contributed instructions for satisfying dependencies on particular platforms.

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
