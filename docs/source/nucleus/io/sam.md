# nucleus.io.sam -- Classes for reading and writing SAM and BAM files.
**Source code:** [nucleus/io/sam.py](https://github.com/google/nucleus/tree/master/nucleus/io/sam.py)
---
API for reading:
  with SamReader(input_path) as reader:
    for read in reader:
      process(reader.header, read)

API for writing:

  with SamWriter(output_path) as writer:
    for read in reads:
      writer.write(read)

where read is a nucleus.genomics.v1.Read protocol buffer.

If the path contains '.tfrecord', a TFRecord file is assumed; otherwise
it is treated as a true SAM file.  Also, file names ending with '.gz'
are assumed to be compressed.

## Classes overview
Name | Description
-----|------------
[`InMemorySamReader`](#inmemorysamreader) | Python interface class for in-memory sam reader.
[`NativeSamReader`](#nativesamreader) | Class for reading from native SAM files.
[`NativeSamWriter`](#nativesamwriter) | Class for writing to native SAM files.
[`SamReader`](#samreader) | Class for reading Read protos from SAM or TFRecord files.
[`SamWriter`](#samwriter) | Class for writing Variant protos to SAM or TFRecord files.

## Classes
### InMemorySamReader
```
Python interface class for in-memory sam reader.

Attributes:
  reads: [nucleus.genomics.v1.Read], the list of in-memory reads.
  is_sorted: bool, if reads are sorted.
```

#### Methods:
<a name="__init__"></a>
#### `__init__(self, reads, is_sorted=False)`

<a name="iterate"></a>
#### `iterate(self)````
Iterate over all records in the reads.

Returns:
  An iterator over nucleus.genomics.v1.Read's.
```

<a name="query"></a>
#### `query(self, region)````
Iterate over records overlapping a query region.

Args:
  region: nucleus.genomics.v1.Range, the query region.

Returns:
  An iterator over nucleus.genomics.v1.Read's.
```

<a name="replace_reads"></a>
#### `replace_reads(self, reads, is_sorted=False)````
Replace the reads stored by this reader.
```

### NativeSamReader
```
Class for reading from native SAM files.

Most users will want to use SamReader instead, because it dynamically
dispatches between reading native SAM files and TFRecord files based
on the filename's extensions.
```

#### Methods:
<a name="__init__"></a>
#### `__init__(self, input_path, use_index=True, read_requirements=None, parse_aux_fields=False, hts_block_size=None, downsample_fraction=None, random_seed=None)````
Initializes a NativeSamReader.

Args:
  input_path: string. A path to a resource containing SAM/BAM records.
    Currently supports SAM text format and BAM binary format.
  use_index: optional bool, defaulting to True. If True, we will attempt to
    load an index file for reads_source to enable the query() API call. If
    True an index file must exist. If False, we will not attempt to load an
    index for reads_source, disabling the query() call.
  read_requirements: optional ReadRequirement proto. If not None, this proto
    is used to control which reads are filtered out by the reader before
    they are passed to the client.
  parse_aux_fields: optional bool. If False, the default, we will not parse
    the auxillary fields of the SAM/BAM records (see SAM spec for details).
    Parsing the aux fields is often unnecessary for many applications, and
    adds a significant parsing cost to access. If you need these aux fields,
    set parse_aux_fields to True and these fields will be parsed and
    populate the appropriate Read proto fields (e.g., read.info).
  hts_block_size: integer or None.  If None, will use the default htslib
    block size.  Otherwise, will configure the underlying block size of the
    underlying htslib file object.  Larger values (e.g. 1M) may be
    beneficial for reading remote files.
  downsample_fraction: None or float in the interval [0.0, 1.0]. If not
    None or 0.0, the reader will only keep each read with probability
    downsample_fraction, randomly.
  random_seed: None or int. The random seed to use with this sam reader, if
    needed. If None, a fixed random value will be assigned.

Raises:
  ValueError: If downsample_fraction is not None and not in the interval
    (0.0, 1.0].
  ImportError: If someone tries to load a tfbam file.
```

<a name="iterate"></a>
#### `iterate(self)`

<a name="query"></a>
#### `query(self, region)`

### NativeSamWriter
```
Class for writing to native SAM files.

Most users will want SamWriter, which will write to either native SAM
files or TFRecords files, based on the output filename's extensions.
```

#### Methods:
<a name="__init__"></a>
#### `__init__(self, output_path, header)````
Initializer for NativeSamWriter.

Args:
  output_path: str. A path where we'll write our SAM/BAM file.
  header: A nucleus.SamHeader protobuf.  The header is used both
    for writing the header, and to control the sorting applied to
    the rest of the file.
```

<a name="write"></a>
#### `write(self, proto)`

### SamReader
```
Class for reading Read protos from SAM or TFRecord files.
```

### SamWriter
```
Class for writing Variant protos to SAM or TFRecord files.
```

