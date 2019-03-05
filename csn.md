# CSN : Comma Separated Notation

This document is the specification for Comma Separated Notation.

## Introduction

The Comma Separated Notation (CSN) is a format for exchanging structured data. The format itself is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support type definitions, instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets.

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
| Reference | Pointer to another record in the payload. |
| Instance | An unit of data that conforms to a Type, Primitive or an Array. |
| Record | A logical unit of data, expressing types, instances, arrays or comments. Record is a set of Fields. |
| Payload | A complete package of data, containing Types, Comments, Instances, Arrays, Primites arranged as per the rules and conventions of CSN defined in this document. |

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
* Fields of a Record are separated by the 'comma' (,) character.
* Record must not contain a new line character.
* Records can be of following types. Each type of Record has its respective rules, which are explained in the following sections:
  * Version
  * Type Definition (TypeDef)
  * Array Definition (ArrayDef)
  * Instance (Data)
* All 'Type' and 'Array' records must the defined before they are used in an other Type, Array or Instance records.
* The first two fields of each record are special as they provide more meaningful information about the record.
  * The first field indicates the type of that record. This field is called the Record Code.
    * A Record Code consists of an Alphabetic character followed by a sequence number.
  * The second field contains information specific to the record itself. More information about this field will be specified with each record type.

#### Version Record

**A Version Record specifies which CSN Standard this document adheres to.**

Eg:
```
V0,'1.0.0'
```

Rules:
*

#### TypeDef Record

**A TypeDef Record is a template which lists its members.**

Eg:
```
T1,'Person','FirstName','LastName'
```

Rules:
* The first field of a TypeDef record must begin with the character 'T' immediately followed by the 'Sequence Number'. (Sequence numbers are explained later in this document).
* The second field specifies a name for this TypeDef. This helps in assigning a logical, readable label to the TypeDef Record.
* Members of the Type are listed from the third field onwards.
* A Type must contain at least one member. There is no value of a Type with no members.
* The Type name (second field) as well its members (remaining fields) are all considered of type 'string'.

#### Array Record

**An Array is a set of values of the same type.**

Eg:
```
T1,'Person','FirstName','LastName'
A2,'Numbers',TI
```

Rules:
* The first field of an Array record must begin with the character 'A' immediately followed by the 'Sequence Number'.
* The second field is a name for the Array.
* The third field is the Record Code for the type of Array elements.
* Array Records contain exactly 3 fields as described above.
* In the above example, it is an Array of type Integer, which is a Primitive Type. Primitive Types are explained later in the document.

#### Instance Record

**An Instance Record is an actual data record of a specific TypeDef.**

Eg:
```
T1,'Person','FirstName','LastName'
A2,'Numbers',TI
I3,T1,'1','1'
I4,T1,'2','2'
I5,A2,100,200
```
Rules:
* The first field of an Instance Record must begin with the character 'I' immediately followed by the 'Sequence Number'.
* The second field is the Record Code of the TypeDef that the respective record represents.
* The data that is part of this record begins third field onwards. These fields (after the second field) are referred to as the "Data Fields" of an Instance.
  * If it is an instance of a Type (i.e. TypeDef record), the sequence of data fields must be as per the sequence of members in the corresponding TypeDef record. Effectively, such an Instance has the same number of fields as the TypeDef record of which it is an instance.
  * If it is an instance of an Array (i.e. ArrayDef record), the data fields must be of the same type as specified in the ArrayDef record. There is no limit on the number of data fields in an Array Instance.

### Sequence Number

**Sequence Number is a mechanism used in CSN to create unique identifiers for records within a Payload.**

#### Type Sequence

#### Instance Sequence

### Field

#### Primitive Types

#### Reference

