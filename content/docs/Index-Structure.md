+++
author = ["Patrick Mézard"]
date = "2015-10-09T00:00:00+01:00"
title = "Index Structure"
[menu.docs]
weight = 30
parent = 'devguide'

+++

Bleve default index, called "upside_down" is stored in a single key/value store
table. The store must be able to enumerate its entries starting at a given key,
in lexicographic byte order.

Index key and values are handled as byte arrays and are called rows (see `index/upside_down/row.go` for details). To store different row types in a single table, bleve prefixes their keys with a single byte, for instance the term frequency keys start with a 't'. The following sections describe the data structures stored in the index with pseudo-Go code for value layout.

### Version Row

Key: `"v"`

Value:
```
struct {
	Version uint8
}
```

Only one version row exists in the index and is used to check version compatibility


### Field Rows

Key: (`"f"`, `fieldIndex`)

Value:
```
struct {
	Name string
}
```

They map field qualified names to internal integer indices. The field names are only used or displayed at API level, bleve uses the indices everywhere internally.


### Dictionary Rows

Key: (`"d"`, `fieldIndex`, `term`)

Value:
```
struct {
	Count uint64
}
```

Dictionary rows index (field, term) pairs and count the number of documents containing them. Range queries over terms of a given field, which includes prefix queries, are implemented by seeking over dictionary rows first.


### Term Frequency Rows

Key: (`"t"`, `fieldIndex`, `term`, `docId`)

Value:
```
type TermFrequencyRow struct {
	Freq    uint64
	Norm    float32
	Vectors []struct{
		Field          uint16
		ArrayPositions []uint64
		Pos            uint64
		Start          uint64
		End            uint64
	}
}
```

Term frequency rows store statistics about a term in a document field. Applications using result highlighting or phrase search must also record term locations by setting `IncludeTermVectors` in the field mapping. Term locations are used to retrieve the term origin in the source document or measure terms proximity.

* `Freq` is the number of occurrences of the term in the field.
* `Norm` is 1/sqrt(number of tokens in the field). This factor helps balancing scores of shorter documents which have lower term frequencies.
* `Vectors` is filled if `IncludeTermVectors` is set in the field mapping. Each element describes a single occurrence of the term in the source document. `Field` is the field index, and is the same as in the row key except for composite fields where it references the source field (composite fields like `_all` are made of the union of other fields). Because documents are structured as trees and intermediate elements can be arrays or slices, the field index is sometimes not enough to resolve a value. The indices of the value or some of its ancestors into those arrays or slices are also required. They are stored in `ArrayPositions`. `Pos` tells the entry is the `Pos`-th occurrence of the term in the field (starting at 1). `Start` and `End` are the byte offsets of the source of the term in the field value.

### Back Index Rows

Key: (`"b"`, `docId`)

Value:
```
struct {
    Indexed []struct{
		Term []byte
		Field uint32
	},
	Stored []struct{
		Field uint32
		ArrayPositions []uint64
	},
```

Back index rows describe the indexed and stored terms of a document. `Field` is the field index and `ArrayPositions` is explained in Term frequency rows section.


### Stored Rows

Key: (`"s"`, `docId`, `fieldId`, `arrayPositions`)

Value:
```
struct {
	Value []byte
}
```

Stored rows contains the values of document fields which mapping had the `Store` property. `arrayPositions` meaning is described in Term frequency rows section.


### Internal Rows

Key: (`"i"`, `key`)

Value:
```
struct {
    Value []byte
}
```

Internal rows are used to store arbitrary data in the index.
