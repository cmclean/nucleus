# nucleus.util.sequence_utils -- Utility functions for manipulating DNA sequences.

## **Source code:** [nucleus/util/sequence_utils.py](https://github.com/google/nucleus/tree/master/nucleus/util/sequence_utils.py)

## Classes overview

Name              | Description
----------------- | -----------
[`Error`](#error) |

## Functions overview

| Name                                                   | Description         |
| ------------------------------------------------------ | ------------------- |
| [`reverse_complement`](#reverse_complement)`(sequence, | Returns the reverse |
: complement_dict=None)`                                 : complement of a DNA :
:                                                        : sequence.           :

## Classes

### Error

## Functions

### reverse_complement

`reverse_complement(sequence, complement_dict=None)`

Returns the reverse complement of a DNA sequence.

By default this will successfully reverse complement sequences comprised solely
of A, C, G, and T letters. Other complement dictionaries can be passed in for
more permissive matching.

**Args**:

`sequence`: The input sequence to reverse complement.

`complement_dict`: The lookup dictionary holding the complement base pairs.

**Returns**:

The reverse complement DNA sequence.

**Raises**:

`Error`: The sequence contains letters not present in complement_dict.
