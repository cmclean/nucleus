# nucleus.io.bed -- Class for reading BED files.
**Source code:** [nucleus/io/bed.py](https://github.com/google/nucleus/tree/master/nucleus/io/bed.py)

[Documentation index](../../doc_index.md)
---
API for reading:
  with BedReader(input_path) as reader:
    for record in reader:
      print(record)

where `record` is a nucleus.genomics.v1.BedRecord protocol buffer.

If the path contains '.tfrecord' as an extension, a TFRecord file is
assumed. Otherwise, it is treated as a true BED file. In either case,
an extension of '.gz' will cause the file to be treated as compressed.

## Classes overview
Name | Description
-----|------------
[`BedReader`](#bedreader) | Class for reading BedRecord protos from BED or TFRecord files.
[`BedWriter`](#bedwriter) | Class for writing BedRecord protos to BED or TFRecord files.
[`NativeBedReader`](#nativebedreader) | Class for reading from native BED files.
[`NativeBedWriter`](#nativebedwriter) | Class for writing to native BED files.

## Classes
### BedReader
```
Class for reading BedRecord protos from BED or TFRecord files.
```

### BedWriter
```
Class for writing BedRecord protos to BED or TFRecord files.
```

### NativeBedReader
```
Class for reading from native BED files.

Most users will want to use BedReader instead, because it dynamically
dispatches between reading native BED files and TFRecord files based on the
filename's extension.
```

#### Methods:
<a name="__init__"></a>
##### `__init__(self, input_path, num_fields=0)`
```
Initializes a NativeBedReader.

Args:
  input_path: string. A path to a resource containing BED records.
  num_fields: int. The number of fields to read in the BED. If unset or set
    to zero, all fields in the input are read.
```

<a name="iterate"></a>
##### `iterate(self)`
```
Returns an iterable of BedRecord protos in the file.
```

<a name="query"></a>
##### `query(self)`


### NativeBedWriter
```
Class for writing to native BED files.

Most users will want BedWriter, which will write to either native BED
files or TFRecord files, based on the output filename's extension.
```

#### Methods:
<a name="__init__"></a>
##### `__init__(self, output_path, header=None)`
```
Initializer for NativeBedWriter.

Args:
  output_path: str. The path to which to write the BED file.
  header: nucleus.genomics.v1.BedHeader. The header that defines all
    information germane to the constituent BED records.
```

<a name="write"></a>
##### `write(self, proto)`


