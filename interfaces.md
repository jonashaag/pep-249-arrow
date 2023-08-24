# Interfaces of existing implementations

## Turbodbc

```py
def fetcharrowbatches(self, strings_as_dictionary=False, adaptive_integers=False):
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

def fetchallarrow(self, ...):
    """
    [... same args ...]

    :return: ``pyarrow.Table``
    """
```
