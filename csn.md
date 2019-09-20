# CSN : Comma Separated Notation

The Comma Separated Notation (CSN) is a format for exchanging structured data between systems. The notation is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple types, instances, arrays and references. CSN is also optimized for size, generation and parsing especially for larger data sets. All information in a CSN format is represented as text.

## Versioning

The CSN Specification will follow the Semantic Versioning 2.0.0 system. Typically, Patch numbers will address errors in the specification, Minor numbers will add non-breaking functionality while Major numbers will represent large changes to the feature set.

The version of this specification is **0.2.0**.

## Reference Documents

* CSV RFC 4180: https://tools.ietf.org/html/rfc4180
* Swagger Specification: https://swagger.io/specification/
* ABNF RFC 2234: https://tools.ietf.org/html/rfc2234

# The CSN Format

## Basic Concepts & Sample

| Concept | Description |
| --- | --- |
| Field | A Field is the smallest unit of information. A Field contains 'atomic' values. |
| Record | A Record is a set of related Fields, that form one logical unit of information. |
| Payload | A Payload is a set of Records, which collectively represent the complete dataset/ information that is being exchanged between the systems. |

Following CSN sample is used throughout the document:
```
0,"0.1.0"
1,"Person","FirstName","LastName"
2,#1,"Bat","Man"
3,#1,"Iron","Man"
4,A,#2,#3
5,A,100,200
```

## Payload

A Payload is a set of Records. Payload is the complete data set that is being exchanged between two systems. Information in a Payload is arranged and organized into Records.

Rules:
* Consecutive records must be separated by the 'line feed' (\n) character.
* Last record must not end with the 'line feed' character.

The complete sample shown above is a Payload.

## Record

A Record is a set of Fields. A Record is a single logical unit of information. It consists of a set of related values, each stored in a distinct Field.

Rules:
* Fields of a Record are separated by the 'comma' (,) character.
* There is no comma character at the beginning or the end of a Record.

In the above sample, each line is a Record.

## Field

A Field is a single, atomic unit of information. Fields store individual values; they are the smallest unit of data/ information in CSN.

Rules
* Fields can not contain a 'comma' or a 'line feed' character, except for the data types where such characters can be 'escaped' (such as String).

In the above sample, individual values on each line (separated by the comma character) are Fields.

A Field can contain 2 types of values:
* a data value expressed as one of the primitive types, OR
* a reference to another Record.

### Primitives

Primitives are the basic data types for presenting values and information. There are additional rules and conventions for representing the values.

| Primitive | Description & Rules |
| --- | --- |
| Boolean | <p>Indicate True or False value</p><ul><li>A 'true' value is indicated with the character 'T' (without the quotes).</li><li>A 'false' value is indicated with the character 'F' (without the quotes).</li></ul> |
| DateTime | <p>Represents a date and time</p><ul><li>DateTime values must start with the character 'D'.</li><li>This must be followed by a date and time value in the ISO 8601 derived basic format as represented by this expression: 'YYYYMMDDThhmmssfff'.</li><li>Timezone information is not part of this field type. It can be accomodated as separate string fields.</li></ul> |
| String | <p>Any literal value that is supported by the character set.</p><ul><li>String values must always be enclosed in Double Quotes (").</li><li>Within the quotes, a string field can contain the comma and the new line characters, as they wouldn't affect the Payload Structure.</li><li>String values can contain the following characters by using a backslash for escaping:<ul><li>&quot; (double quote)</li><li>`\` (back slash)</li></ul></li></ul> |
| Real Number | <p>Represent any numerical value, including fractions.</p><ul><li>Use a period (.) for a decimal separator.</li><li>There must be no thousand separators in the number.</li><li>Use hyphen/ minus character for negative numbers at the beginning of the number.</li><li>Exponentiation formats are not yet supported.</li></ul> |
| Integer | <p>Represent numbers without fractions.</p><ul><li>Integer must not have a decimal or a thousand separator.</li><li>Use hyphen/ minus character for negative numbers.</li></ul> |

Sample with all Primitives as elements of an Array:
```
0,"0.2.0"
1,A,T,F,D20190312T192433567,"Label",-123.45,345
```

### Reference

A **Reference** is a pointer to another record in the Payload. It achieves this by using the Sequence Number of the target Record. References assist nesting of data structures.

Rules for References:
* Reference values start with the '#' character.
* This is immediately followed by the Sequence Number of the target record which is being referenced.

## Record Structure

### Sequence Number & Record Type

The first 2 Fields of any Record store special information, that reveal additional information about the Record itself. The first Field of any Record is a **Sequence Number**. It is an integer value, starting with 0 (zero) for the first record, and incrementing by 1 for each successive Record. This Sequence Number acts as a unique identifier for the Record within the Payload.

### Record Types
In the CSN Format, Records follow specific rules of organization and structure according to the information that they represent. Accordingly, there are 4 types of Records:
* Version Record
* Type Definition (TypeDef) Record
* Array Record
* Instance Record

### Version Record

A Version Record specifies which CSN Standard this document adheres to. The Version Record is the metadata about the Payload.

Rules:
* A Version Record is the first record of the Payload.
* There can be only one Version record in a Payload.
* A Version Record contains exactly 2 fields:
  * Its Sequence Number is always "0" (without the quotes) as it is always the first record of a Payload.
  * The second field states the version of the CSN specification as a string type.

In the above sample, the first line is the Version Record.

### TypeDef Record

A TypeDef Record is a template which lists the labels/ names for the Fields of an Instance Record.

Rules:
* The first field in the TypeDef Record is the Sequence Number.
* The second field specifies a human readable name for this TypeDef.
* Members of the Type are listed from the third field onwards.
* The Type name (second field) as well its members (following the second field) are of the data type 'string'.
* A TypeDef Record must be defined before it is Referenced in the Payload.

In the above sample, the second line defines a type called 'Person'.

### Instance Record

An Instance Record is an actual data record containing relevant and important values.

Rules:
* The first field is the Sequence Number.
* The second field is the Reference of the TypeDef that the Instance adheres to.
* The data that is part of this record begins third field onwards. These fields are referred to as the "Data Fields" of an Instance.
* The sequence of data fields should be as per the sequence of members in the corresponding TypeDef record. Effectively, an Instance should have the same number of fields as its TypeDef.

In the above sample, the third and fourth lines are Instance Records. They are Instances of the type defined in the second line.

### Array Record

An Array Record is quite simply a set of values. 

Rules:
* The first field is the Sequence Number.
* The second field is the literal 'A' (without the quotes), which indicates that this Record is an Array.
* Third field onwards are actual values, which are elements of the array.
* There is no limit on the number of fields in an Array.
* The elements of an array may be of different types.

In the above sample, Record with Sequence Number 4 is an Array of References to Instances while Record with Sequence Number 5 is an Array of Integers.

### Null (Missing) Values

If some values are missing in an Instance or an Array Record, they can be left blank. This is similar to the concept of "null" in several platforms and programming languages.

Eg: Null fields in the middle and at the end of a Record.
```
0,"0.2.0"
1,"NullCheck","Null1","NotNull","Null2"
2,#1,,"not null",
```

## Syntax vs Semantics

The CSN Specification is strict about the syntax, but not so much about the semantics. There are some generally accepted programming language semantics, which CSN chooses not to adhere to, allowing the application to enforce its own rules.

* Heterogenous Array Elements
  The Array Record doesn't define a type for Elements. Contrast this with the Instance Record, which always indicates the TypeDef record as part of its own structure. In a typical programming language platform, the Array is usually a data structure containing a set of homogenous elements.
* TypeDef vs Instance Fields Count Mismatch
  An Instance of a TypeDef should, ideally, have the same number of data fields as the members in the TypeDef. However, CSN currently doesn't enforce it strictly for correctness of a CSN Payload. 

## ABNF Notation:

```
Payload = VersionRecord *(LF NonVersionRecord)

VersionRecord = VersionRecordCode FieldSep VersionString
FieldSep = ","
VersionRecordCode = "V0"
VersionString = "0.1.0"

NonVersionRecord = (TypeDef / Array / Instance)
```
