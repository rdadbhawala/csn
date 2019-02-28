# CSN : Comma Separated Notation

This document is the specification for Comma Separated Notation.

## Introduction

The Comma Separated Notation (CSN) is a format for exchanging structured data. The format itself is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple type definitions, multiple instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets.

### Versioning

The CSN Specification will follow the Semantic Versioning 2.0.0 system. Typically, Patch numbers will address errors in the specification, Minor numbers will add non-breaking functionality while Major numbers will represent large changes to the feature set.

The version of this specification is **1.0.0**.

### Concepts

|     |     |
| --- | --- |
| Field | The smallest unit of data, expressing some value. |
| Primitive | A basic data type supported in the specification. |
| Array | A set of values of the same type. |
| Type | A complex data type having Primitives, Arrays or other Types as its Members. |
| Comment | A remark or a statement, typically not part of data. |
| Reference | Pointer to another record in the payload. |
| Instance | An unit of data that conforms to a Type, Primitive or an Array. |
| Record | A logical unit of data, expressing types, instances, arrays or comments. Record is a set of Fields. |
| Payload | A complete package of data, containing Types, Comments, Instances, Arrays, Primites arranged as per the rules and conventions of CSN defined in this document. |
| Version Record | First record of the payload, that represents version information of the CSN format. |

## Specification

This section explains how data is structured/ organized in the CSN format. It follows a top-down approach, as it will introduce the largest concepts first, and then go on to the smaller ones.

### Payload

**Payload is a set of Records**

Eg:
```
Record
Record
```

Rules:
* Consecutive records are separated by the 'new line' (\n) character.
* Last Record must not end with the 'new line' character.

### Record

**Record is a set of Fields.**

Eg:
```
Field,Field,Field
```

Rules:
* Fields are separated by the 'comma' (,) character.
* Record must not contain a new line character.
* Records can be of following types. Each type of Record has its respective rules, which are explained in the following sections.
  * Type Definition
  * Array Definition
  * Instance
* All 'Type' and 'Array' records must the defined before they are used in an other Type, Array or Instance records.
* The first field of each record is special as it indicates the Type of that record.

#### Type Record

**A Type Record lists its members.**

Eg:
```
T1,'Person','FirstName','LastName','DateOfBirth'
```

Rules:
* The first field of a Type record must begin with the character 'T' immediately followed by the 'Type Sequence Number'. This field is henceforth refered to as the 'Type-Code' in the rest of this document.
* The second field is a name for the Type.
* Members of the Type are listed third field onwards.
* A Type must contain at least one member. There is no value of a Type with no members.
* The Type name (second field) as well its members (remaining fields) are all considered of type 'string'.

#### Array Record

**An Array is a set of values of the same type.**

Eg:
```
T1,Person,FirstName,LastName,DateOfBirth
A2,Persons,T1
```

Rules:
* The first field of an Array record must begin with the character 'A' immediately followed by the 'Type Sequence Number'.
* The second field is a name for the Array.
* The third field is the type-code for the type of Array elements.
* Array Records contain exactly 3 fields as described above.


#### Instance Record

### Sequence Numbers

#### Type Sequence

#### Instance Sequence

### Field

#### Primitive Types

#### Reference

