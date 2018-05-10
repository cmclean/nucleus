# nucleus.util.cigar -- Utility functions for working with alignment cigars.
**Source code:** [nucleus/util/cigar.py](https://github.com/google/nucleus/tree/master/nucleus/util/cigar.py)
---


## Functions overview
Name | Description
-----|------------
[`alignment_length`](#alignment_length)`(cigar_units)` | Computes the span in basepairs of the cigar units.
[`format_cigar_units`](#format_cigar_units)`(cigar_units)` | Returns the string version of an iterable of CigarUnit protos.
[`parse_cigar_string`](#parse_cigar_string)`(cigar_str)` | Parse a cigar string into a list of cigar units.
[`to_cigar_unit`](#to_cigar_unit)`(source)` | Creates a cigar_pb2 CigarUnit from source.
[`to_cigar_units`](#to_cigar_units)`(source)` | Converts object to a list of CigarUnit.

## Functions
### `alignment_length(cigar_units)`<a name="alignment_length"></a>
```python
Computes the span in basepairs of the cigar units.

Args:
  cigar_units: iterable[CigarUnit] whose alignment length we want to compute.

Returns:
  The number of basepairs spanned by the cigar_units.
```

### `format_cigar_units(cigar_units)`<a name="format_cigar_units"></a>
```python
Returns the string version of an iterable of CigarUnit protos.

Args:
  cigar_units: iterable[CigarUnit] protos.

Returns:
  A nucleus.genomics.v1.Range for read.
```

### `parse_cigar_string(cigar_str)`<a name="parse_cigar_string"></a>
```python
Parse a cigar string into a list of cigar units.

For example, if cigar_str is 150M2S, this function will return:

[
  CigarUnit(operation=ALIGNMENT_MATCH, operation_length=150),
  CigarUnit(operation=SOFT_CLIP, operation_length=2)
]

Args:
  cigar_str: str containing a valid cigar.

Returns:
  list[cigar_pb2.CigarUnit].

Raises:
  ValueError: If cigar_str isn't a well-formed CIGAR.
```

### `to_cigar_unit(source)`<a name="to_cigar_unit"></a>
```python
Creates a cigar_pb2 CigarUnit from source.

This function attempts to convert source into a CigarUnit protobuf. If
source is a string, it must be a single CIGAR string specification like
'12M'. If source is a tuple or a list, must have exactly two elements
(operation_length, opstr). operation_length can be a string or int, and must
be >= 1. opstr should be a single character CIGAR specification (e.g., 'M').
If source is already a CigarUnit, it is just passed through unmodified.

Args:
  source: many types allowed. The object we want to convert to a CigarUnit
    proto.

Returns:
  CigarUnit proto with operation_length and operation set to values from
    source.

Raises:
  ValueError: if source cannot be converted or is malformed.
```

### `to_cigar_units(source)`<a name="to_cigar_units"></a>
```python
Converts object to a list of CigarUnit.

This function attempts to convert source into a list of CigarUnit protos.
If source is a string, we assume it is a CIGAR string and call
parse_cigar_string on it, returning the result. It not, we assume it's an
iterable containing element to be converted with to_cigar_unit(). The
resulting list of converted elements is returned.

Args:
  source: str or iterable to convert to CigarUnit protos.

Returns:
  list[CigarUnit].
```

