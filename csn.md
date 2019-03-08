# CSN : Comma Separated Notation

This document is the specification for Comma Separated Notation.

## Introduction

The Comma Separated Notation (CSN) is a format for exchanging structured data between systems. The notation is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple types, instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets.

### Versioning

The CSN Specification will follow the Semantic Versioning 2.0.0 system. Typically, Patch numbers will address errors in the specification, Minor numbers will add non-breaking functionality while Major numbers will represent large changes to the feature set.

The version of this specification is **0.1.0**. It is still in beta.

### Reference Documents

* CSV RFC 4180: https://tools.ietf.org/html/rfc4180
* Swagger Specification: https://swagger.io/specification/
* ABNF RFC 2234: https://tools.ietf.org/html/rfc2234

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

## Specification

This section explains how data is structured/ organized in the CSN format. It follows a top-down approach, as it will introduce the larger concepts first, and then go on to the smaller ones.

### Payload

**Payload is a set of Records.**

Payload basically is the complete data set that is being exchanged between two systems. Information in a Payload is arranged and structured according to the rules and conventions defined in this document. 

All information is organized into records. There are 4 types of Records:
* Version Record
* Type Definition (TypeDef) Record
* Array Definition (ArrayDef) Record
* Instance Record

Rules:
* Consecutive records must be separated by the 'line feed' (\n) character.
* Last record must not end with the 'line feed' character.
* The first record must be a Version record.
* Second record onwards, there can be 
  * Zero or more TypeDef records,
  * Zero or more ArrayDef records, and
  * Zero or more Instance Records.

Eg:
```
V0,'1.0.0'
T1,'Person','FirstName','LastName'
A2,'Numbers',PI
I3,T1,'1','1'
I4,T1,'2','2'
I5,A2,100,200
```

### Record

**A Record is a set of Fields.**

A Record is a single logical unit of data. It consist of a set of values, with each value recorded in a Field.

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
  * Instance
* All 'Type' and 'Array' records must the defined before they are used in an other Type, Array or Instance records.
* The first two fields of each record are special as they provide more meaningful information about the record.
  * The first field indicates the type of that record. This field is called the Record Code. It consists of an alphabetic code followed by a Sequence Number.
  * The second field contains information specific to the record itself. More information about this field will be specified with each record type.

#### Version Record

**A Version Record specifies which CSN Standard this document adheres to.**

Eg:
```
V0,'1.0.0'
```

Rules:
* A Version Record is the first record of the Payload.
* A Version Record contains exactly 2 fields.
  * The first field, which is the Record Code, is always "V0".
  * The second field states the version of the CSN specification.
* There can be only one Version Record in a Payload.

#### TypeDef Record

**A TypeDef Record is a template which lists its members.**

Eg:
```
V0,'1.0.0'
T1,'Person','FirstName','LastName'
```

Rules:
* The first field of a TypeDef record must begin with the character 'T' immediately followed by the 'Sequence Number'. (Sequence numbers are explained later in this document).
* The second field specifies a name for this TypeDef. This helps in assigning a logical, readable label to the TypeDef Record.
* Members of the Type are listed from the third field onwards.
* A Type must contain at least one member. There is no value of a Type with no members.
* The Type name (second field) as well its members (remaining fields) are all considered of type 'string'.

#### ArrayDef Record

**An Array is a set of values of the same type. An ArrayDef Record indicates an array type.**

Eg:
```
V0,'1.0.0'
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
V0,'1.0.0'
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

### Record Codes

**As described above, a Record Code is a unique identifier for the Record.**

It consists of 2 parts:
* An alphabetic code which identifies the type of record.
* A Sequence Number, which is a unique identifier.

#### Alphabetic Codes

**The alphabetic codes reveal the type of record.**

Following are the allowed alphabetic codes:
| Alphabet | Description |
| --- | --- |
| V | Version |
| T | TypeDef |
| A | ArrayDef |
| I | Instance |
| P | Primitive Types |

#### Sequence Number

**Sequence Number is a mechanism used in CSN to create unique identifiers for each record within a Payload.**

Eg:
```
V0,'1.0.0'
T1,'Person','FirstName','LastName'
A2,'Numbers',PI
I3,T1,'1','1'
I4,T1,'2','2'
I5,A2,100,200
```

Rules:
* Sequence Numbers are integers.
* Sequence Numbers start with 0, then increment by 1 for each successive record.
* The Sequence Number of a Record by itself can be a unique identifier (without the alphabetic code).

### Field

**A Field is the smallest unit of data/ information in a Payload.**

A Field can contain 2 types of values:
* a data value expressed as one of the primitive types, OR
* a reference to an Instance, a TypeDef or an ArrayDef Record.

Eg:
```
```

Rules
* Fields can not contain a 'comma' or a 'new line' character, except for the string type where such characters can be 'escaped'.
* Additional rules for Primitive Types and References are given in the following section.

#### Primitive Types

Primitive Types are the basic data types supported in the CSN notation for representing data, information and values. The supported Primitive Types are:
* String
* Boolean
* (Real) Number
* (Long) Integer
* DateTime

##### String

**A String Type can store any literal value that is supported by the character set.**

Rules:
* String values must always be enclosed in Single Quotes (').
* Within the single quotes, a string field can contain the comma and the new line characters.
* String values can contain the following characters in an escaped manner:

| Character | Escape |
| --- | --- |
| `'` (single quote) | `\'` | 
| `\` (back slash) | `\\` |

##### Boolean

**A Boolean type reflects a True or a False value.**

Rules:
* A 'true' value is indicated with the character 'Y' (without the quotes).
* A 'false' value is indicated with the character 'N' (without the quotes).

##### Floating Point Number

**A Floating Point Number can represent any numerical value.**

Rules:
* Use a period (.) for a decimal separator.
* No thousand separators.
* Use hyphen/ minus character for negative numbers.
* Exponentiation formats are not yet supported.

##### Integer

**An Integer represents numbers without fractions.**

Rules:
* Integer doesn't use a decimal or a thousand separator.
* Use hyphen/ minus character for negative numbers.

##### DateTime

**A DateTime value represents a date and time.**

Rules:
* Timestamp values must start with the character 'D'.
* This must be followed by a date and time value in the ISO 8601 Format.
* The date and time must be expressed in the 'basic' format expressed as 'YYYYMMDDThhmmss.sss±hhmm' for localized time, or as 'YYYYMMDDThhmmss.sssZ' for UTC time.

#### Reference

**References are pointers to other records in the Payload.**

References can reduce redundancy in the Payload.

Rules:

* Reference values start with the '#' character.
* This is immediately followed by the Sequence Number of the target record which is being referenced.

## ABNF Notation:

```
Payload = VersionRecord *(LF NonVersionRecord)
LF = Line Feed Character
NonVersionRecord = (TypeDefRecord / ArrayDefRecord / InstanceRecord)
```

