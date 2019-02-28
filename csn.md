# CSN : Comma Separated Notation

This document is the specification for Comma Separated Notation.

## Introduction

The Comma Separated Notation (CSN) is a format for exchanging structured data. The format itself is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple type definitions, multiple instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets.

### Versioning

The CSN Specification will follow the Semantic Versioning 2.0.0 system. Typically, Patch numbers will address errors in the specification, Minor numbers will add non-breaking functionality while Major numbers will represent large changes to the feature set.

The version of this specification is ** 1.0.0 **.

### Concepts

|     |     |
| --- | --- |
| Primitive | A basic data type supported in the specification. |
| Array | A set of values of the same type. |
| Type | A complex data type having Primitives, Arrays or other Types as its Members. |
| Comment | A remark or a statement, typically not part of data. |
| Field | The smallest unit of data, expressing some value. |
| Reference | Pointer to another record in the payload. |
| Instance | An unit of data that conforms to a Type, Primitive or an Array. |
| Record | A logical unit of data, expressing types, instances, arrays or comments. Record is a set of Fields. |
| Payload | A complete package of data, containing Types, Comments, Instances, Arrays, Primites arranged as per the rules and conventions of CSN defined in this document. |
| Version Record | First record of the payload, that represents version information of the CSN format. |

## Specification

This section explains how data is structured/ organized in the CSN format. It follows a top-down approach, as it will introduce the largest concepts first, and then go on to the smaller ones.

### Payload

** Definition: ** Payload is a set of Records, separated by the "new line" (\n) character.

Eg:
```
Record
Record

```

Rules:
* Payload is expressed in the UTF-8 Character Set.
* First record of the Payload must be the Version Record.
* Last record must also be followed by the "new line" character.

### Record

** Definition: ** Record is a set of Fields, separated by the 'comma' (,) character.

Eg:
```
Field,Field,Field
```

Rules:
* Record must not contain a new line character, except for a field of 'string' type which can contain the new line character.

#### Version Record

