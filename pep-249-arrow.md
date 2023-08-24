**ChatGPT Prompt:**

> Write a Python Enhancement Proposal that extends PEP 249 (Python Database API Specification v2.0) as follows:
> Define two new methods on Cursor objects:
> - `fetchall_arrow()`: Fetch all (remaining) rows of a query result, returning them as a single pyarrow.Table. Implementations do not need to take into account the cursor's arraysize attribute. Error handling is equivalent to the `fetchall()` method.
> - `fetchmany_arrow([size])`: Fetch the next set of rows of a query result, returning a pyarrow.Table. An empty pyarrow.Table is returned when no more rows are available. The number of rows to fetch per call is specified by the parameter.  If it is not given, the number of rows to be fetched may be chosen freely by the implementation. The method should try to fetch as many rows as indicated by the size parameter. If this is not possible due to the specified number of rows not being available, fewer rows may be returned. Error handling is equivalent to the `fetchmany()` method.

TODO:

- `apilevel_arrow`
- Do we want to specify any Arrow specific exceptions?
- `description_arrow`: `name`, `arrow_type`.
- Mandatory column types: Do we want to specify some mandatory type mappings? Eg. SQL int64 is always mapped to Arrow's int64. We should try a couple of implementations to see what types they return.
- 

# PEP XXXX: Extension of PEP 249 - Python Database API Specification v2.0 with Arrow Support

## Abstract

This PEP proposes an extension to the Python Database API Specification v2.0 (PEP 249) to include support for Apache Arrow. Two new methods are proposed to be added to the Cursor class: `fetchall_arrow()` and `fetchmany_arrow([size])`.

## Rationale

Apache Arrow is a cross-language development platform for in-memory data. It specifies a standardized language-independent columnar memory format for flat and hierarchical data, organized for efficient analytic operations on modern hardware. The Arrow Python library, PyArrow, provides Python APIs to this platform. 

By extending the Python Database API to include methods that return PyArrow tables, we can leverage the efficiency of Arrow's columnar data structures, and facilitate interoperability with other systems that use Arrow.

## Specification

### The `fetchall_arrow()` Method

The `fetchall_arrow()` method fetches all (remaining) rows of a query result, returning them as a single `pyarrow.Table`. Implementations do not need to take into account the cursor's `arraysize` attribute. Error handling is equivalent to the `fetchall()` method.

```python
def fetchall_arrow(self) -> pyarrow.Table: ...
```

### The `fetchmany_arrow([size])` Method

The `fetchmany_arrow([size])` method fetches the next set of rows of a query result, returning a `pyarrow.Table`. An empty `pyarrow.Table` is returned when no more rows are available. The number of rows to fetch per call is specified by the `size` parameter. If it is not given, the number of rows to be fetched may be chosen freely by the implementation. The method should try to fetch as many rows as indicated by the `size` parameter. If this is not possible due to the specified number of rows not being available, fewer rows may be returned. Error handling is equivalent to the `fetchmany()` method.

```python
def fetchmany_arrow(self, size: Optional[int] = None) -> pyarrow.Table: ...
```

## Backwards Compatibility

This PEP is fully backwards compatible as it only introduces new methods.

## References

- [PEP 249 -- Python Database API Specification v2.0](https://www.python.org/dev/peps/pep-0249/)
- [Apache Arrow and PyArrow](https://arrow.apache.org/docs/python/index.html)

## Copyright

This document has been placed in the public domain.
