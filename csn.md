# CSN : Comma Separated Notation

This document is the specification for Comma Separated Notation.

## Introduction

The Comma Separated Notation (CSN) is a format for exchanging structured data between systems. The notation is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple types, instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets. All information in a CSN format is represented as text.

### Versioning

The CSN Specification will follow the Semantic Versioning 2.0.0 system. Typically, Patch numbers will address errors in the specification, Minor numbers will add non-breaking functionality while Major numbers will represent large changes to the feature set.

The version of this specification is **0.1.0**.

### Reference Documents

* CSV RFC 4180: https://tools.ietf.org/html/rfc4180
* Swagger Specification: https://swagger.io/specification/
* ABNF RFC 2234: https://tools.ietf.org/html/rfc2234

## The CSN Format

### Basic Concepts & Sample


| Concept | Description |
| --- | --- |
| Field | A Field is the smallest unit of information. |
| Record | A Record is a set of related Fields, that form one logical unit of information. |
| Payload | A Payload is a set of Records, which collectively represent the complete dataset/ information that is being exchanged between the systems. |

Following CSN sample is used throughout the document:
```
V0,"0.1.0"
T1,"Person","FirstName","LastName"
I2,#1,"Bat","Man"
I3,#1,"Iron","Man"
A4,#1,#2,#3
A5,PI,100,200
```

### Payload

Payload is a set of Records. Payload is the complete data set that is being exchanged between two systems. Information in a Payload is arranged and organized into Records. 

Basic Rules for Payloads:
* Consecutive records must be separated by the 'line feed' (\n) character.
* Last record must not end with the 'line feed' character.

The complete sample in the above example is a Payload.

### Record

Record is a set of Fields. A Record is a single logical unit of information. It consists of a set of values, each stored in a distinct Field.

Rules:
* Fields of a Record are separated by the 'comma' (,) character.
* There is no comma character at the beginning or the end of a Record.

In the above sample, each line is a Record.

### Field

A Field is a single, atomic unit of information. Fields store individual values; they are the smallest unit of data/ information in a Payload.

Rules
* A Field can contain 2 types of values:
  * a data value expressed as one of the primitive types, OR
  * a reference to another Record (discussed later in the document).
* Fields can not contain a 'comma' or a 'line feed' character, except for the data types where such characters can be 'escaped' (such as String).

In the above sample, individual values on each line (separated by the comma character) are Fields.

### Record Code & References

In the CSN Format, Records follow specific rules of organization and structure according to the information that they represent. These are the types of Records:
* Version Record
* Type Definition (TypeDef) Record
* Array Record
* Instance Record

In order to correctly identify the type of data stored in the Record, as well as assign it a unique identifier, the first field of any Record must be a valid **Record Code**. It consists of 2 parts:
* An Alphabetic code which identifies the type of record, and
* A Sequence Number which is a unique identifier.

**Alphabetic Codes** reveal the type of record. Particularly in implementation, the Alphabetic Code will aid in setting up parsers for efficient execution. Following are the allowed alphabetic codes for each type of Record:

| Alphabet | Description |
| --- | --- |
| V | Version |
| T | TypeDef |
| A | Array |
| I | Instance |

**Sequence Number** is a numbering system used to create unique identifiers for each Record within a Payload. Sequence Numbers are integers. They start with 0 and increment by 1 for each successive record.

**References** are pointers to other records in the Payload. References can reduce redundancy in the Payload. They can help to easily overcome cyclical references within data. References use the Sequence Number to point to specific Records within the Payload.

Rules for References:
* Reference values start with the '#' character.
* This is immediately followed by the Sequence Number of the target record which is being referenced.
* Note that the Alphabetic Code is not included in the Reference.

### Primitives

Primitives are the basic data types for presenting values and information. There are additional rules and conventions for representing the values.

| Primitive | Description & Rules |
| --- | --- |
| Boolean | <p>Indicate True or False value</p><ul><li>A 'true' value is indicated with the character 'T' (without the quotes).</li><li>A 'false' value is indicated with the character 'F' (without the quotes).</li></ul> |
| DateTime | <p>Represents a date and time</p><ul><li>DateTime values must start with the character 'D'.</li><li>This must be followed by a date and time value in the ISO 8601 Format.</li><li>The date and time must be expressed in the 'basic' format expressed as 'YYYYMMDDThhmmss.sss±hhmm' for localized time, or as 'YYYYMMDDThhmmss.sssZ' for UTC time.</li></ul> |
| String | <p>Any literal value that is supported by the character set.</p><ul><li>String values must always be enclosed in Double Quotes (").</li><li>Within the quotes, a string field can contain the comma and the new line characters, as they wouldn't affect the Payload Structure.</li><li>String values can contain the following characters by using a backslash for escaping:<ul><li>&quot; (double quote)</li><li>`\` (back slash)</li></ul></li></ul> |
| Real Number | <p>Represent any numerical value, including fractions.</p><ul><li>Use a period (.) for a decimal separator.</li><li>There must be no thousand separators in the number.</li><li>Use hyphen/ minus character for negative numbers at the beginning of the number.</li><li>Exponentiation formats are not yet supported.</li></ul> |
| Integer | <p>Represent numbers without fractions.</p><ul><li>Integer must not have a decimal or a thousand separator.</li><li>Use hyphen/ minus character for negative numbers.</li></ul> |

Sample with all Primitives:
```
V0,"0.1.0"
T1,"AllPrimitives","BooleanTrue","BooleanFalse","DateTime","String","Real","Integer"
I2,#1,T,F,D20190312T192433.567Z,"Label",-123.45,345
```

### Version Record

A Version Record specifies which CSN Standard this document adheres to. The Version Record is the metadata about the Payload.

Rules:
* A Version Record is the first record of the Payload.
* There can be only one Version record in a Payload.
* A Version Record contains exactly 2 fields:
  * Its Record Code is always "V0" (without the quotes) as it is always the first record of a Payload.
  * The second field states the version of the CSN specification as a string type.

In the above sample, the first line is the Version Record.

### TypeDef Record

A TypeDef Record is a template which lists the labels/ names for the Fields.

Rules:
* The first field Record Code starts with Alphabetic Code 'T' followed by the Sequence Number.
* The second field specifies a human readable name for this TypeDef.
* Members of the Type are listed from the third field onwards.
* The Type name (second field) as well its members (following the second field) are of the data type 'string'.
* A TypeDef Record must be defined before it is Referenced in a Payload.

In the above sample, the second line defines a type called 'Person'.

### Instance Record

An Instance Record is an actual data record containing relevant and important values.

Rules:
* The first field Record Code starts with Alphabetic Code 'I' followed by the Sequence Number.
* The second field is the Reference of the TypeDef that the Instance adheres to.
* The data that is part of this record begins third field onwards. These fields are referred to as the "Data Fields" of an Instance.
* The sequence of data fields must be as per the sequence of members in the corresponding TypeDef record. Effectively, an Instance has the same number of fields as its TypeDef.

In the above sample, the third and fourth lines are Instance Records. They are Instances of the type defined in the second line.

### Array Record

An Array Record is a set of values of the same type.

Rules:
* The first field Record Code starts with Alphabetic Code 'A' followed by the Sequence Number.
* The second field indicates the type of elements in the Array.
* Third field onwards are actual values, which are elements of the array.
* If it is an Array of Primitive Types (explained later in this document):
  * the second field indicates the type, and
  * the values are given directly in the record.
* If it is an Array of Instances of TypeDef Record:
  * the second field is a reference to the TypeDef Record, and
  * the values are expressed as reference to Instances.
* There is no limit on the number of fields in an Array Instance.

In the above sample, A4 is an array of Instances while A5 is an Array of Integers.

To present an Array of primitive types, Primitive Codes must be used.

| Primitive Type | Primitive Code |
| --- | --- |
| Boolean | PB |
| DateTime | PD |
| String | PS |
| Real Number | PF |
| Integer | PI |

### Null (Missing) Values

If some values are missing in an Instance Record, they can be left blank. This is similar to the concept of "null" in several platforms and programming languages.

Eg: Null fields in the middle and at the end of a Record.
```
V0,"0.1.0"
T1,"NullCheck","Null1","NotNull","Null2"
I2,#1,,"not null",
```

## ABNF Notation:

```
Payload = VersionRecord *(LF NonVersionRecord)

VersionRecord = VersionRecordCode FieldSep VersionString
FieldSep = ","
VersionRecordCode = "V0"
VersionString = "0.1.0"

NonVersionRecord = (TypeDef / Array / Instance)
```
