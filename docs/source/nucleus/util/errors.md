# nucleus.util.errors -- Library of application-specific errors.
**Source code:** [nucleus/util/errors.py](https://github.com/google/nucleus/tree/master/nucleus/util/errors.py)
---


## Classes overview
Name | Description
-----|------------
[`CommandLineError`](#commandlineerror) | Exception class related to invalid command-line flags.
[`Error`](#error) | Base class for core error types.

## Functions overview
Name | Description
-----|------------
[`clean_commandline_error_exit`](#clean_commandline_error_exit)`(allowed_exceptions=(Error, CommandLineError), exit_value=errno.ENOENT)` | Wraps commands to capture certain exceptions and exit without stacktraces.
[`log_and_raise`](#log_and_raise)`(msg, exception_class=Error)` | Logs the given message at ERROR level and raises exception.

## Classes
### CommandLineError
```python
Exception class related to invalid command-line flags.
```

### Error
```python
Base class for core error types.
```

## Functions
### `clean_commandline_error_exit(allowed_exceptions=(Error, CommandLineError), exit_value=errno.ENOENT)`<a name="clean_commandline_error_exit"></a>
```python
Wraps commands to capture certain exceptions and exit without stacktraces.

This function is intended to wrap all code within main() of Python binaries
to provide a mechanism for user errors to exit abnormally without causing
exceptions to be thrown. Any exceptions that are subclasses of those listed
in `allowed_exceptions` will be caught and the program will quietly exit with
`exit_value`. Other exceptions are propagated normally. It should only be used
as a context manager and its usage should be limited to main().

Args:
  allowed_exceptions: [`tuple of Exception`]. A tuple of Exception classes
      that should not be raised, but instead quietly caused to exit the
      program.
  exit_value: [`int`]. The value to return upon program exit.

Yields:
  The yield in this function is used to allow the block nested in the with
  statement to be executed.
```

### `log_and_raise(msg, exception_class=Error)`<a name="log_and_raise"></a>
```python
Logs the given message at ERROR level and raises exception.

Args:
  msg: [`string`]. The message to log and use in the raised exception.
  exception_class: [`Exception`]. The class of exception to raise.

Raises:
  Error: An exception of the type specified by the input exception_class.
```

