PEP: 249-1
Title: Arrow extensions to PEP 249
Author: Jonas Haag <jonas@lophus.org>
Status: Draft
Type: Standards Track
Content-Type: text/x-rst
Created: 01-Sep-2023


Abstract
========

It is proposed to add an optional extension to :pep:`249` (Python Database API Specification v2.0) to support fetching rows as ``pyarrow.Table``.
Namely, this PEP proposes to add the following methods and attributes to the ``Cursor`` object:

- ``arrow_fetchall()``
- ``arrow_fetchmany([size])``
- ``arrow_apilevel``

Motivation
==========

`Apache Arrow <https://arrow.apache.org>`_ offers a language-agnostic, columnar memory format for representing tables.
When fetching a large number of rows from a database, using the Arrow format over Python types can yield signifcant memory and compute savings.

Arrow is the de-facto standard for cross-language interchange of tabular data. It is supported as an export format by most Python database drivers and dataframe libraries.
However, the interface offered by Python database drivers to fetch results in Arrow format is not standardized.

The goal of this PEP is to standardize this interface so that programs that use Arrow-enabled databases do not have to implement bespoke support for each individual database driver.


Specification
=============

Database driveres that support fetching data in the Arrow format should expose the following attributes and methods in the `Cursor <https://peps.python.org/pep-0249/#cursor-objects>`_ object:

- ``arrow_apilevel``: An attribute analogous to PEP 249's ``apilevel``. It always has the value ``"1.0"``.
- ``arrow_fetchall()``: A method that fetches all (remaining) rows of a query result, returning them as a single ``pyarrow.Table``.
  Implementations do not need to take into account the cursor's ``arraysize`` attribute.
- ``arrow_fetchmany([size])``: A method that fetches the next set of rows of a query result, returning a ``pyarrow.Table``.
  An empty ``pyarrow.Table`` is returned when no more rows are available.
  The number of rows to fetch per call is specified by the ``size`` parameter. If it is not given, the number of rows to be fetched may be chosen freely by the implementation.
  The method should try to fetch as many rows as indicated by the size parameter. If this is not possible due to the specified number of rows not being available, fewer rows may be returned.

Mixing Arrow and non-Arrow interfaces
-------------------------------------

Users of the Arrow API should not mix the Arrow and non-Arrow interfaces.
Calling ``arrow_fetch*`` after a call to ``fetch*`` on the same ``Cursor``, and vice-versa, raises an exception.


Non-Goals
=========

The methods specified in this PEP may freely choose the column data types of the returned ``pyarrow.Table`` for efficiency reasons.
The column types may generally be chosen to be different from the backing database's column types.
As an example, when fetching a subset of rows from a 64-bit integer database column that only contains integers that fit into 32 bits, the database driver may use a 32-bit integer column type in the returned ``pyarrow.Table``.


Backwards compatibility
=======================

The proposed additions are fully backwards compatible with :pep:`249`.


Copyright
=========

This document has been placed in the public domain.
