# nucleus.io.genomics_writer -- Abstract base class for objects writing genomics data.
**Source code:** [nucleus/io/genomics_writer.py](https://github.com/google/nucleus/tree/master/nucleus/io/genomics_writer.py)
---
Most users will want to use a subclass of GenomicsWriter, with code like
the following:

  with GenomicsWriterSubClass(output_path, options) as writer:
    for proto in records:
      writer.write(proto)

## Classes overview
Name | Description
-----|------------
[`DispatchingGenomicsWriter`](#dispatchinggenomicswriter) | A GenomicsWriter that dispatches based on the file extension.
[`GenomicsWriter`](#genomicswriter) | Abstract base class for writing genomics data.
[`TFRecordWriter`](#tfrecordwriter) | A GenomicsWriter that writes to a TFRecord file.

## Classes
### DispatchingGenomicsWriter
```
A GenomicsWriter that dispatches based on the file extension.

If '.tfrecord' is present in the filename, a TFRecordWriter is used.
Otherwise, a native writer is.

Sub-classes of DispatchingGenomicsWriter must define a _native_writer()
method.
```

#### Methods:
<a name="__init__"></a>
#### `__init__(self, output_path, **kwargs)`

<a name="write"></a>
#### `write(self, proto)`

### GenomicsWriter
```
Abstract base class for writing genomics data.

A GenomicsWriter only has one method, write, which writes a single
protocol buffer to a file.
```

#### Methods:
<a name="write"></a>
#### `write(self, proto)````
Writes proto to the file.

Args:
  proto:  A protocol buffer.
```

### TFRecordWriter
```
A GenomicsWriter that writes to a TFRecord file.
```

#### Methods:
<a name="__init__"></a>
#### `__init__(self, output_path, header=None)`

<a name="write"></a>
#### `write(self, proto)`

