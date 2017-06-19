# lunr-schema

**WORK IN PROGRESS**

JSONSchema definition of the format required for a Lunr index.

This is intended to describe and define the format of a serialised Lunr index. By defining the schema it is hoped that other backends will be able to generate a Lunr compatible schema.

## Schema

The current expected format for Lunr 2.x is a single JSON object with the following properties:

* `version`
* `fields`
* `fieldVectors`
* `invertedIndex`
* `pipeline`

### Version

Currently this is just a string with the version of Lunr that created the index. This may change to the version of the schema being used.

## Fields

A list of the names of the indexed fields

## Field Vectors

In Lunr documents are split into fields. There will be a vector for every field in every document. Document fields are stored as a vector space, where each token in the index has a specific dimension, and the value of that dimension is the weight of that token in that field/document.

Calculating the weight is _probably_ an implementation detail, but a higher value indicates that the token is more important to that document than a lower weight.

Vectors are represented as a single array. This is specific to the implementation of vectors within Lunr. Both the dimension and values are stored in the same array, though they should be considered a flattened list of tuples.

```
  [[3, 1.23], [8, 0.98], [17, 3.21]] => [3, 1.23, 8, 0.98, 17, 3.21]
```

So the even indexed values are the dimensions, and the odd indexed values are the weights.

## Inverted Index

A list of tuples of term to index entries. This is a list so that no sorting needs to be done during deserialisation. As such it **must** be sorted by the term. If not sorted it will cause errors during deserialisation.

### Index Entry

An index entry is an JSON object that **must** contain a property named `_index` which should be unique for this term. It is the dimension used in the vector space representation.

The other properties should be field names, and the values are posting objects.

### Posting

A posting is a map from document reference to metadata. By default no metadata is captured during indexing and so the metadata can be an empty object. Otherwise this is a key value store of metadata key to metadata value.

## Pipeline

An ordered list of pipeline function names that should be added to the search pipeline. By default this will include just the stemmer as "stemmer".
