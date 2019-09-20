# CSN: Comma Separated Notation

The Comma Separated Notation (CSN) is a format for exchanging structured data between systems. The notation is strongly influenced by the Comma Separated Values (CSV) format but the specification adds enhancements to support multiple types, instances, arrays and references. CSN is optimized for size as well as generation and parsing, especially for larger data sets. CSN is a 'textual' format.

## The Specification 
The format is described in the specification document [here](csn.md).

## Objectives

### Multiple Types, Arrays & References
When two systems communicate, they often exchange a complex data structure of information. This involves 
* pieces of data that are of distinct structure (unique types),
* multiple pieces of the same data structure arranged together as a single logical unit (arrays), and
* references to a specific data structure in the full payload, especially for nested data structures

CSN proposes to represent all such data structures.

### Size over Performance over Readability
CSN prioritizes the Size over other features. It minimizes the overheads in the format to the bare minimum, thus reducing the proportion of meta data versus the data in the overall payload. Next, inherently due to the nature of the format, CSN addresses the Parsing and Generation performance. Readability is deliberately given the lowest priority. CSN treats readability as a 'visualization' problem, rather than as a problem of the format itself.

### Medium to Large Datasets
CSN presents itself as a suitable candidate for medium to large sized payloads. Extremely small payloads may be better represented in other formats. Since CSN optimizes the overheads in the format, as the size of data increases, greater is the gain in using the CSN format.

