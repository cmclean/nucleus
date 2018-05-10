# nucleus.io.fastq -- Class for reading FASTQ files.
**Source code:** [nucleus/io/fastq.py](https://github.com/google/nucleus/tree/master/nucleus/io/fastq.py)
---
API for reading:
  with FastqReader(input_path) as reader:
    for record in reader:
      print(record)

API for writing:
  with FastqWriter(output_path) as writer:
    for record in records:
      writer.write(record)

where `record` is a nucleus.genomics.v1.FastqRecord protocol buffer.

If the path contains '.tfrecord' as an extension, a TFRecord file is
assumed.  Otherwise, it is treated as a true FASTQ file.  In either case,
an extension of '.gz' will cause the file to be treated as compressed.

## Classes overview
Name | Description
-----|------------
[`FastqReader`](#fastqreader) | Class for reading FastqRecord protos from FASTQ or TFRecord files.
[`FastqWriter`](#fastqwriter) | Class for writing FastqRecord protos to FASTQ or TFRecord files.
[`NativeFastqReader`](#nativefastqreader) | Class for reading from native FASTQ files.
[`NativeFastqWriter`](#nativefastqwriter) | Class for writing to native FASTQ files.

## Classes
### FastqReader
```
Class for reading FastqRecord protos from FASTQ or TFRecord files.
```

### FastqWriter
```
Class for writing FastqRecord protos to FASTQ or TFRecord files.
```

### NativeFastqReader
```
Class for reading from native FASTQ files.

Most users will want to use FastqReader instead, because it dynamically
dispatches between reading native FASTQ files and TFRecord files based on the
filename's extension.
```

#### Methods:
<a name="__init__"></a>
##### `__init__(self, input_path)`
```
Initializes a NativeFastqReader.

Args:
  input_path: string. A path to a resource containing FASTQ records.
```

<a name="iterate"></a>
##### `iterate(self)`
```
Returns an iterable of FastqRecord protos in the file.
```

<a name="query"></a>
##### `query(self)`


### NativeFastqWriter
```
Class for writing to native FASTQ files.

Most users will want FastqWriter, which will write to either native FASTQ
files or TFRecord files, based on the output filename's extension.
```

#### Methods:
<a name="__init__"></a>
##### `__init__(self, output_path)`
```
Initializer for NativeFastqWriter.

Args:
  output_path: str. The path to which to write the FASTQ file.
```

<a name="write"></a>
##### `write(self, proto)`


