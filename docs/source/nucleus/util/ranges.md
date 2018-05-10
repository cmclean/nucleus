# nucleus.util.ranges -- Utilities for Range overlap detection.
**Source code:** [nucleus/util/ranges.py](https://github.com/google/nucleus/tree/master/nucleus/util/ranges.py)
---


## Classes overview
Name | Description
-----|------------
[`RangeSet`](#rangeset) | Fast overlap detection of a genomic position against a db of Ranges.

## Functions overview
Name | Description
-----|------------
[`as_tuple`](#as_tuple)`(range_)` | Returns a Python tuple (reference_name, start, end).
[`bed_parser`](#bed_parser)`(filename)` | Parses Range objects from a BED-formatted file object.
[`bedpe_parser`](#bedpe_parser)`(filename)` | Parses Range objects from a BEDPE-formatted file object.
[`contigs_dict`](#contigs_dict)`(contigs)` | Creates a dictionary for contigs.
[`contigs_n_bases`](#contigs_n_bases)`(contigs)` | Returns the sum of all n_bases of contigs.
[`expand`](#expand)`(region, n_bp, contig_map=None)` | Expands region by n_bp in both directions.
[`find_max_overlapping`](#find_max_overlapping)`(query_range, search_ranges)` | Gets the index of the element in search_ranges with max overlap with query.
[`from_regions`](#from_regions)`(regions, contig_map=None)` | Parses each region of `regions` into a Range proto.
[`length`](#length)`(region)` | Returns the length in basepairs of region.
[`make_position`](#make_position)`(chrom, position, reverse_strand=False)` | Makes a nucleus.genomics.v1.Position.
[`make_range`](#make_range)`(chrom, start, end)` | Creates a genomics.Range object chr:start-end.
[`overlap_len`](#overlap_len)`(range1, range2)` | Computes the number of overlapping bases of range1 and range2.
[`parse_literal`](#parse_literal)`(region_literal, contig_map=None)` | Parses a Range from a string representation like chr:start-end.
[`parse_literals`](#parse_literals)`(region_literals, contig_map=None)` | Parses each literal of region_literals in order.
[`position_overlaps`](#position_overlaps)`(chrom, pos, interval)` | Does interval overlap the position chr:pos?
[`ranges_overlap`](#ranges_overlap)`(i1, i2)` | Checks whether ranges i1 and i2 overlap.
[`sorted_ranges`](#sorted_ranges)`(ranges, contigs=None)` | Sorts ranges by reference_name, start, and end.
[`span`](#span)`(regions)` | Returns a region that spans all of the bases in regions.
[`to_literal`](#to_literal)`(range_pb)` | Converts Range protobuf into string literal form.

## Classes
### RangeSet
```python
Fast overlap detection of a genomic position against a db of Ranges.

Enables very fast O(log n) computation of whether a point chr:pos falls within
one of a large number of genomic ranges.

This class does not supports overlapping or adjacent intervals. Any such
intervals will be automatically merged together in the constructor.

This class is immutable. No methods should be added that directly modify the
ranges held by the class.
```

#### Methods:
#### `__init__(self, ranges=None, contigs=None)`<a name="__init__"></a>
```python
Creates an RangeSet backed by ranges.

Note that the Range objects in ranges are *not* stored directly here, so
they can safely be modified after they are passed to this RangeSet.

Args:
  ranges: A list of genomics.Range objects (or anything with
    reference_name, start, and end properties following the
    genomics.Range convention). If None, no ranges will be used,
    and overlaps() will always return False.
  contigs: a list of ContigInfo protos, used to define the iteration order
    over contigs (i.e., by contig.pos_in_fasta).  If this dict is not
    provided, the iteration order will be determined by the alphabetical
    order of the contig names.
Raises:
  ValueError: if any range's reference_name does not correspond to any
    contig in `contigs`.
```

#### `exclude_regions(self, other)`<a name="exclude_regions"></a>
```python
Chops out all of the intervals in other from this this RangeSet.

This is a *MUTATING* operation for performance reasons. Make a copy of self
if you want to avoid modifying the RangeSet.

Args:
  other: A RangeSet object whose intervals will be removed from this
    RangeSet.
```

#### `from_bed(cls, source, contigs=None)`<a name="from_bed"></a>
```python
Creates a RangeSet containing the intervals from source.

Args:
  source: A path to a BED (or equivalent) file of intervals.
  contigs: An optional list of ContigInfo proto, used by RangeSet
    constructor.

Returns:
  A RangeSet.
```

#### `from_contigs(cls, contigs)`<a name="from_contigs"></a>
```python
Creates a RangeSet with an interval covering each base of each contig.
```

#### `from_regions(cls, regions, contig_map=None)`<a name="from_regions"></a>
```python
Parses a command-line style literal regions flag into a RangeSet.

Args:
  regions: An iterable or None. If not None, regions will be parsed with
    ranges.from_regions.
  contig_map: An optional dictionary mapping from contig names to ContigInfo
    protobufs. If provided, allows literals of the format "contig_name",
    which will be parsed into a Range with reference_name=contig_name,
    start=0, end=n_bases where n_bases comes from the ContigInfo;
    additionally the sort order of the RangeSet will be determined by
    contig.pos_in_fasta.

Returns:
  A RangeSet object.
```

#### `intersection(self, *others)`<a name="intersection"></a>
```python
Computes the intersection among this RangeSet and *others RangeSets.

This function computes the intersection of all of the intervals in self and
*others, returning a RangeSet containing only intervals common to all. The
intersection here is an ranged intersection, not an identity intersection,
so the resulting set of intervals may not contain any of the original
intervals in any of the sets.

To be concrete, suppose we have three sets to intersect, each having two
intervals:

  self   : chr1:1-10, chr2:20-30
  other1 : chr1:5-8, chr3:10-40
  other2 : chr1:3-7, chr3:10-30

self.intersection(other1, other2) produces a RangeSet with one interval
chr1:5-7, the common bases on chr1 in self, other1, and other2. No intervals
on chr2 or chr3 are included since the chr2 only occurs in self and the two
intervals on chr3, despite having some shared bases, don't have an
overlapping interval in self.

Args:
  *others: A list of RangeSet objects to intersect with the intervals in
    this RangeSet.

Returns:
  A RangeSet. If *others is empty, this function returns self rather than
  making an unnecessary copy. In all other cases, the returned value will be
  a freshly allocated RangeSet.
```

#### `overlaps(self, chrom, pos)`<a name="overlaps"></a>
```python
Returns True if chr:pos overlaps with any range in this RangeSet.

Uses a fast bisection algorithm to determine the overlap in O(log n) time.

Args:
  chrom: The chromosome name as a string.
  pos: The position (0-based) as an integer.

Returns:
  True if chr:pos overlaps with a range.
```

#### `partition(self, max_size)`<a name="partition"></a>
```python
Splits our intervals so that none are larger than max_size.

Slices up the intervals in this RangeSet into a equivalent set of interval (
i.e., spanning the same set of bases), each of which is at most max_size in
length.

This function does not modify this RangeSet.

Because RangeSet merges adjacent intervals, this function cannot return use
a RangeSet to represent the partitioned intervals and so instead generates
these intervals via a yield statement.

Args:
  max_size: A positive integer (> 0) indicating the maximum size of any
    interval.

Yields:
  nucleus.genomics.v1.Range protos, in sorted order (see comment about order
  in __iter__).

Raises:
  ValueError: if max_size <= 0.
```

#### `variant_overlaps(self, variant, empty_set_return_value=True)`<a name="variant_overlaps"></a>
```python
Returns True if the variant's range overlaps with any in this set.
```

## Functions
### `as_tuple(range_)`<a name="as_tuple"></a>
```python
Returns a Python tuple (reference_name, start, end).
```

### `bed_parser(filename)`<a name="bed_parser"></a>
```python
Parses Range objects from a BED-formatted file object.

See http://bedtools.readthedocs.org/en/latest/content/general-usage.html
for more information on the BED format.

Args:
  filename: file name of a BED-formatted file.

Yields:
  nucleus.genomics.v1.Range protobuf objects.
```

### `bedpe_parser(filename)`<a name="bedpe_parser"></a>
```python
Parses Range objects from a BEDPE-formatted file object.

See http://bedtools.readthedocs.org/en/latest/content/general-usage.html
for more information on the BEDPE format.

Skips events that span across chromosomes. For example, if the starting
location is on chr1 and the ending location is on chr2, that record will
not appear in the output.

Args:
  filename: file name of a BEDPE-formatted file.

Yields:
  nucleus.genomics.v1.Range protobuf objects.
```

### `contigs_dict(contigs)`<a name="contigs_dict"></a>
```python
Creates a dictionary for contigs.

Args:
  contigs: Iterable of ContigInfo protos.

Returns:
  A dictionary mapping contig.name: contig for each contig in contigs.
```

### `contigs_n_bases(contigs)`<a name="contigs_n_bases"></a>
```python
Returns the sum of all n_bases of contigs.
```

### `expand(region, n_bp, contig_map=None)`<a name="expand"></a>
```python
Expands region by n_bp in both directions.

Takes a Range(chrom, start, stop) and returns a new
Range(chrom, new_start, new_stop), where:

-- new_start is max(start - n_bp, 0)
-- new_stop is stop + n_bp if contig_map is None, or min(stop + n_bp, max_bp)
   where max_bp is contig_map[chrom].n_bp.

Args:
  region: A nucleus.genomics.v1.Range proto.
  n_bp: int >= 0; how many basepairs to increase region by.
  contig_map: None, or dict[string, ContigInfo]. If not None, used to get the
    maximum extent to increase stop by. Must have region.reference_name as a
    key.

Returns:
  nucleus.genomics.v1.Range proto.

Raises:
  ValueError: if n_bp is invalid.
  KeyError: contig_map is not None and region.reference_name isn't a key.
```

### `find_max_overlapping(query_range, search_ranges)`<a name="find_max_overlapping"></a>
```python
Gets the index of the element in search_ranges with max overlap with query.

In case of ties, selects the lowest index range in search_ranges.

Args:
  query_range: learning.genomics.genomics.core.Range, read genomic range.
  search_ranges: list[learning.genomics.genomics.core.Read]. Cannot be an
    iterable as we loop over the search_ranges multiple times. The list of
    regions we want to search for the maximal overlap with query_range.

Returns:
  int, the search_ranges index with the maximum read overlap. Returns None
  when read has no overlap with any of the search_ranges or search_ranges is
  empty.
```

### `from_regions(regions, contig_map=None)`<a name="from_regions"></a>
```python
Parses each region of `regions` into a Range proto.

This function provides a super high-level interface for
reading/parsing/converting objects into Range protos. Each `region` of
`regions` is processed in turn, yielding one or more Range protos. This
function inspects the contents of `region` to determine how to convert it to
Range(s) protos. The following types of `region` strings are supported:

  * If region ends with an extension known in _get_parser_for_file, we treat
    region as a file and read the Range protos from it with the corresponding
    reader from _get_parser_for_file, yielding each Range from the file in
    order.
  * Otherwise we parse region as a region literal (`chr20:1-10`) and return
    the Range proto.

Args:
  regions: iterable[str]. Converts each element of this iterable into
    region(s).
  contig_map: An optional dictionary mapping from contig names to ContigInfo
    protobufs. If provided, allows literals of the format "contig_name",
    which will be parsed into a Range with reference_name=contig_name,
    start=0, end=n_bases where n_bases comes from the ContigInfo.

Yields:
  A Range proto.
```

### `length(region)`<a name="length"></a>
```python
Returns the length in basepairs of region.
```

### `make_position(chrom, position, reverse_strand=False)`<a name="make_position"></a>
```python
Makes a nucleus.genomics.v1.Position.
```

### `make_range(chrom, start, end)`<a name="make_range"></a>
```python
Creates a genomics.Range object chr:start-end.

Args:
  chrom: The chromosome name as a string.
  start: The start position (0-based, inclusive, integer) of this range.
  end: The end position (0-based, exclusive, integer) of this range.

Returns:
  A nucleus.genomics.v1.Range.
```

### `overlap_len(range1, range2)`<a name="overlap_len"></a>
```python
Computes the number of overlapping bases of range1 and range2.

Args:
  range1: learning.genomics.genomics.Range.
  range2: learning.genomics.genomics.Range.

Returns:
  int. The number of basepairs in common. 0 if the ranges are not on the same
  contig.
```

### `parse_literal(region_literal, contig_map=None)`<a name="parse_literal"></a>
```python
Parses a Range from a string representation like chr:start-end.

The region literal must conform to the following pattern:

  chromosome:start-end
  chromosome:position
  chromosome  [if contig_map is provided]

chromosome can be any non-empty string without whitespace. start and end must
both be positive integers. They can contain commas for readibility. start and
end are positions not offsets, so start == 1 means an offset of zero. If only
a single position is provided, this creates a 1 bp interval starting at
position - 1 and ending at position.

Inspired by the samtools region specification:
http://www.htslib.org/doc/samtools.html

Args:
  region_literal: string literal to parse.
  contig_map: An optional dictionary mapping from contig names to ContigInfo
    protobufs. If provided, allows literals of the format "contig_name", which
    will be parsed into a Range with reference_name=contig_name, start=0,
    end=n_bases where n_bases comes from the ContigInfo.

Returns:
  nucleus.genomics.v1.Range.

Raises:
  ValueError: if region_literal cannot be parsed.
```

### `parse_literals(region_literals, contig_map=None)`<a name="parse_literals"></a>
```python
Parses each literal of region_literals in order.
```

### `position_overlaps(chrom, pos, interval)`<a name="position_overlaps"></a>
```python
Does interval overlap the position chr:pos?

Args:
  chrom: The chromosome name as a string.
  pos: The position (0-based, integer).
  interval: nucleus.genomics.v1.Range object.

Returns:
  True if interval overlaps chr:pos.
```

### `ranges_overlap(i1, i2)`<a name="ranges_overlap"></a>
```python
Checks whether ranges i1 and i2 overlap.

Args:
  i1: nucleus.genomics.v1.Range object.
  i2: nucleus.genomics.v1.Range object.

Returns:
  True if i1 and i2 overlap.
```

### `sorted_ranges(ranges, contigs=None)`<a name="sorted_ranges"></a>
```python
Sorts ranges by reference_name, start, and end.

Args:
  ranges: A sequence of google.v1.genomics.Range protos that we want to sort.
  contigs: None or an iterable of ConfigInfo protos. If not None, we will use
    the order of the contigs (as defined by their pos_in_fasta field values)
    to sort the Ranges on different contigs with respect to each other.

Returns:
  A newly allocated list of google.v1.genomics.Range protos.
```

### `span(regions)`<a name="span"></a>
```python
Returns a region that spans all of the bases in regions.

This function returns a Range(chrom, start, stop), where start is the min
of the starts in regions, and stop is the max end in regions. It may not be
freshly allocated.

Args:
  regions: list[Range]: an list of Range protos.

Returns:
  Range proto.

Raises:
  ValueError: if not all regions have the same reference_name.
  ValueError: if regions is empty.
```

### `to_literal(range_pb)`<a name="to_literal"></a>
```python
Converts Range protobuf into string literal form.

The string literal form looks like:

  reference_name:start+1-end

since start and end are zero-based inclusive (start) and exclusive (end),
while the literal form is one-based inclusive on both ends.

Args:
  range_pb: A nucleus.genomics.v1.Range object.

Returns:
  A string.
```
