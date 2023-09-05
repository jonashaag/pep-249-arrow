# Interfaces of existing implementations

## Turbodbc

```py
def fetcharrowbatches(self, strings_as_dictionary=False, adaptive_integers=False) -> Generator[pyarrow.Table]:
    """
    Fetches rows in the active result set generated with ``execute()`` or
    ``executemany()`` as an iterable of arrow tables.

    :param strings_as_dictionary: If true, fetch string columns as
             dictionary[string] instead of a plain string column.

    :param adaptive_integers: If true, instead of the integer type returned
            by the database (driver), this produce integer columns with the
            smallest possible integer type in which all values can be
            stored. Be aware that here the type depends on the resulting
            data.

    :return: generator of ``pyarrow.Table``
    """

def fetchallarrow(self, ...) -> pyarrow.Table:
    """(Same args as fetcharrowbatches.)"""
```

## Snowflake Connector Python

```py
def fetch_arrow_batches(self) -> Iterator[Table]: ...
def fetch_arrow_all(self) -> Table | None: ...
```

## ADBC

```py
def fetchallarrow(self) -> pyarrow.Table: ...
def fetch_arrow_table(self) -> pyarrow.Table: ...
def fetch_record_batch(self) -> pyarrow.RecordBatchReader: ...
```

## ConnectorX

```py
def read_sql(
    conn: Union[str, Dict[str, str]],
    query: Union[List[str], str],
    *,
    return_type: str = "pandas",
    ...
) -> pyarrow.Table | ...others... :
```

## DuckDB

```py
def fetch_arrow_table(self, rows_per_batch: int = ...) -> pyarrow.Table: ...
def fetch_record_batch(self, rows_per_batch: int = ...) -> pyarrow.RecordBatchReader: ...
```

## Databricks

```py
def fetchall_arrow(self) -> pyarrow.Table: ...
def fetchmany_arrow(self, size) -> pyarrow.Table:  # Fetches the next batch
```

## BigQuery

```py
def to_arrow_iterable(self, ...) -> Iterator[pyarrow.RecordBatch]:
def to_arrow(self, ...) -> pyarrow.Table:
```

## arrow-odbc-py

```py
def read_arrow_batches_from_odbc(...) -> BatchReader (= Iterator[pyarrow.RecordBatch])
```
