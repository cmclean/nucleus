# nucleus.io.genomics_reader -- Abstract base class for objects reading genomics data.
**Source code:** [nucleus/io/genomics_reader.py](https://github.com/google/nucleus/tree/master/nucleus/io/genomics_reader.py)
---
Most users will want to use a subclass of GenomicsReader, with code like
the following:

  with GenomicsReaderSubClass(output_path, options) as reader:
    for proto in reader:
      do_something(reader.header, proto)

## Classes overview
Name | Description
-----|------------
[`DispatchingGenomicsReader`](#dispatchinggenomicsreader) | A GenomicsReader that dispatches based on the file extension.
[`GenomicsReader`](#genomicsreader) | Abstract base class for reading genomics data.
[`TFRecordReader`](#tfrecordreader) | A GenomicsReader that reads from a TFRecord file.

## Classes
### DispatchingGenomicsReader
```python
A GenomicsReader that dispatches based on the file extension.

If '.tfrecord' is present in the filename, a TFRecordReader is used,
otherwise a native reader is.

Sub-classes of DispatchingGenomicsReader must define the following methods:
  * _native_reader()
  * _record_proto()
```

#### Methods:
#### `__init__(self, input_path, **kwargs)`<a name="__init__"></a>


#### `iterate(self)`<a name="iterate"></a>


#### `query(self, region)`<a name="query"></a>


### GenomicsReader
```python
Abstract base class for reading genomics data.

In addition to the abstractmethods defined below, sub-classes should
also set a .header member variable in their objects.
```

#### Methods:
#### `__init__(self)`<a name="__init__"></a>
```python
Allows users to use the object as an iterator.
```

#### `iterate(self)`<a name="iterate"></a>
```python
Returns an iterator for going through the file's records.
```

#### `query(self, region)`<a name="query"></a>
```python
Returns an iterator for going through the records in the region.

Args:
  region:  A nucleus.genomics.v1.Range.

Returns:
  An iterator.
```

### TFRecordReader
```python
A GenomicsReader that reads from a TFRecord file.

Example usage:
  reader = TFRecordReader('/tmp/my_file.tfrecords.gz',
                          proto=tensorflow.Example)
  for example in reader:
    process(example)

Note that TFRecord files do not have headers, and do not need
to be wrapped in a "with" block.
```

#### Methods:
#### `__init__(self, input_path, proto, tf_options=None)`<a name="__init__"></a>
```python
Initializes the TFRecordReader.

Args:
  input_path:  The filename of the file to read.
  proto:  The protocol buffer type the TFRecord file is expected to
    contain.  For example, variants_pb2.Variant or reads_pb2.Read.
  tf_options:  A python_io.TFRecordOptions object.  If not set,
    __init__ will create one with the compression type based on
    whether input_path ends in '.gz' or not.
```

#### `iterate(self)`<a name="iterate"></a>


#### `query(self, region)`<a name="query"></a>

