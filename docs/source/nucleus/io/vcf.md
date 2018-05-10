# nucleus.io.vcf -- Classes for reading and writing VCF files.
**Source code:** [nucleus/io/vcf.py](https://github.com/google/nucleus/tree/master/nucleus/io/vcf.py)
---
API for reading:
  with VcfReader(input_path) as reader:
    for variant in reader:
      process(reader.header, variant)

API for writing:
  with VcfWriter(output_path, header) as writer:
    for variant in variants:
      writer.write(variant)

where variant is a nucleus.genomics.v1.Variant protocol buffer.

If the path contains '.tfrecord', then a TFRecord file is assumed.
Otherwise, it is treated as a true VCF file.  In either case, an
extension of '.gz' will cause the file to be treated as compressed.

## Classes overview
Name | Description
-----|------------
[`InMemoryVcfReader`](#inmemoryvcfreader) | Class for "reading" Variant protos from an in-memory cache of variants.
[`NativeVcfReader`](#nativevcfreader) | Class for reading from native VCF files.
[`NativeVcfWriter`](#nativevcfwriter) | Class for writing to native VCF files.
[`VcfHeaderCache`](#vcfheadercache) | This class creates a cache of accessors to structured fields in Variants.
[`VcfReader`](#vcfreader) | Class for reading Variant protos from VCF or TFRecord files.
[`VcfWriter`](#vcfwriter) | Class for writing Variant protos to VCF or TFRecord files.

## Classes
### InMemoryVcfReader
Class for "reading" Variant protos from an in-memory cache of variants.

API:
  variants = [... Variant protos ...]
  header = variants_pb2.VcfHeader()
  with InMemoryVcfReader(variants, header) as reader:
    for variant in reader:
      process(reader.header, variant)

This class accepts a collection of variants and optionally a header and
provides all of the standard API functions of VcfReader but instead of
fetching variants from a file the variants are queried from an in-memory cache
of variant protos.

Note that the input variants provided to this class aren't checked in any way,
and their ordering determines the order of variants emitted by this class for
iterate and query operation. This is intentional, to make this class easy to
use for testing where you often want to use less-than-perfectly formed inputs.
In order to fully meet the contract of a standard VcfReader, variants should
be sorted by their contig ordering and then by their start and finally by
their ends.

Implementation note:
  The current implementation will be very slow for query() if the provided
  cache of variants is large, as we do a O(n) search to collect all of the
  overlapping variants for each query. There are several straightforward
  optimizations to do if we need/want to scale this up. (a) sort the variants
  and use a binary search to find overlapping variants (b) partition the
  variants by contig, so we have dict[contig] => [variants on contig], which
  allows us to completely avoid considering any variants on any other contigs.
  Neither of these optimizations are worth it if len(variants) is small, but
  it may be worth considering if we want to use this functionality with a
  large number of variants.

#### Methods:
####<a name="<_ast.FunctionDef object at 0x55f78d0f3f50>"></a> __init__(self, variants, header=None)
Creates a VCFReader backed by a collection of variants.

**Args**:

`variants`: list of nucleus.genomics.v1.Variant protos we will "read"
    from.

`header`: a VCFHeader object to provide as a result to calls to self.header,
    or None, indicating that we don't have a header associated with this
    reader.


####<a name="<_ast.FunctionDef object at 0x55f78d1ba5d0>"></a> iterate(self)


####<a name="<_ast.FunctionDef object at 0x55f78d1ba7d0>"></a> query(self, region)


### NativeVcfReader
Class for reading from native VCF files.

Most users will want to use VcfReader instead, because it dynamically
dispatches between reading native VCF files and TFRecord files based
on the filename's extensions.

#### Methods:
####<a name="<_ast.FunctionDef object at 0x55f78d0d99d0>"></a> __init__(self, input_path, use_index=True, excluded_info_fields=None, excluded_format_fields=None)
Initializer for NativeVcfReader.

**Args**:

`input_path`: str. The path to the VCF file to read.

`use_index`: bool. If True, the input is assumed to be bgzipped and tabix
    indexed, and the `query` functionality is supported.

`excluded_info_fields`: list(str). A list of INFO field IDs that should not
    be parsed into the Variants. If None, all INFO fields are included.

`excluded_format_fields`: list(str). A list of FORMAT field IDs that should
    not be parsed into the Variants. If None, all FORMAT fields are
    included.


####<a name="<_ast.FunctionDef object at 0x55f78d1b74d0>"></a> iterate(self)


####<a name="<_ast.FunctionDef object at 0x55f78d1b7690>"></a> query(self, region)


### NativeVcfWriter
Class for writing to native VCF files.

Most users will want VcfWriter, which will write to either native VCF
files or TFRecords files, based on the output filename's extensions.

#### Methods:
####<a name="<_ast.FunctionDef object at 0x55f78d0f7350>"></a> __init__(self, output_path, header=None, round_qualities=False, excluded_info_fields=None, excluded_format_fields=None)
Initializer for NativeVcfWriter.

**Args**:

`output_path`: str. A path where we'll write our VCF file.

`header`: nucleus.genomics.v1.VcfHeader. The header that defines all
    information germane to the constituent variants. This includes contigs,
    FILTER fields, INFO fields, FORMAT fields, samples, and all other
    structured and unstructured header lines.

`round_qualities`: bool. If True, the QUAL field is rounded to one point
    past the decimal.

`excluded_info_fields`: list(str). A list of INFO field IDs that should not
    be written to the output. If None, all INFO fields are included.

`excluded_format_fields`: list(str). A list of FORMAT field IDs that should
    not be written to the output. If None, all FORMAT fields are included.


####<a name="<_ast.FunctionDef object at 0x55f78d0d9510>"></a> write(self, proto)


### VcfHeaderCache
This class creates a cache of accessors to structured fields in Variants.

The INFO and FORMAT fields within Variant protos are structured and typed,
with types defined by the corresponding VCF header. This cache object provides
provides {info,format}_field_{get,set}_fn functions that can be used to
extract information from the structured Variant protos based on the types
defined therein.

Note: Users should not need to interact with this class at all. It is used
by the variant_utils.{get,set}_info and variantcall_utils.{get,set}_format
functions for interacting with the INFO and FORMAT fields in a Variant proto.

#### Methods:
####<a name="<_ast.FunctionDef object at 0x55f78d0ea310>"></a> __init__(self, header)
Constructor.

**Args**:

`header`: nucleus.genomics.v1.VcfHeader proto. Used to define the accessor
    functions needed.


####<a name="<_ast.FunctionDef object at 0x55f78d1b9310>"></a> format_field_get_fn(self, field_name)
Returns a callable that gets the given FORMAT field based on its type.

####<a name="<_ast.FunctionDef object at 0x55f78d1b9190>"></a> format_field_set_fn(self, field_name)
Returns a callable that sets the given FORMAT field based on its type.

####<a name="<_ast.FunctionDef object at 0x55f78d1b9c90>"></a> info_field_get_fn(self, field_name)
Returns a callable that extracts the given INFO field based on its type.

**Args**:

`field_name`: str. The INFO field name of interest, e.g. 'AA', 'DB', 'AF'.


**Returns**:

  A callable used to extract the given INFO field from a Variant proto.

####<a name="<_ast.FunctionDef object at 0x55f78d1b9d10>"></a> info_field_set_fn(self, field_name)
Returns a callable that sets the given INFO field based on its type.

### VcfReader
Class for reading Variant protos from VCF or TFRecord files.

### VcfWriter
Class for writing Variant protos to VCF or TFRecord files.

