# pydbtools

[![Build Status](https://travis-ci.org/INWTlab/pydbtools.svg?branch=master)](https://travis-ci.org/INWTlab/pydbtools)

**pydbtools is a python package built for easy use of raw SQL within python and pandas projects.**

It uses ideas from [records](https://github.com/kennethreitz/records/) and is built using [sqlalchemy-engines](https://www.sqlalchemy.org/), but is more heavily integrated with pandas. It aims to reproduce the pilosophy behind the R-package [dbtools](https://github.com/INWT/dbtools/).


_Database support includes RedShift, Postgres, MySQL, SQLite, Oracle, and MS-SQL (drivers not included)._

## Usage

### Send queries and bulk queries

Easy sending of raw sql and output as pandas DataFrames, with creds or the url of the database:

```python
from pydbtools import Database

db = Database(creds=creds)
df = db.send_query("""
    SELECT * FROM test;
    """)
df # table test as pandas DataFrame
```

You can put the sql query in a file and direct `send_query` to the file. The sql-file may be parametrized:

```sql
SELECT {col1}, {col2} FROM test;

```

```python
from pydbtools import Database

db = Database(creds=creds, sql_dir = '/path/to/dir/')
db.send_query('select', col1='id', col2='name')
df # table test, including columns 'id', 'name' as pandas DataFrame
```

You can also pass arguments to pandas `read_sql`-Function:

```python
from pydbtools import Database

db = Database(creds=creds, sql_dir = '/path/to/dir/')
db.send_query('select', col1='id', col2='name', index_col='id')
df # table test, including column 'name' as pandas DataFrame with index 'id'
```

You may also send queries with no table as output to the database via `send_bulk_query`, which exhibits the same behavior as `send_query`:

```python
db.send_bulk_query('drop test from test;')
```

### Send data

Easy sending of pandas Dataframes in multiple modes:

```python
db.send_data(df, 'table', mode='insert')
```

Supported modes are:
  - 'insert': Appending new records. Duplicate primary keys will result in errors (sql insert into).
  - 'truncate': Delete the table and completely rewrite it (sql truncate and insert into).
  - 'replace': Replace records with duplicate primary keys (sql replace into).
  - 'update': Update records with duplicate primary keys (sql insert into duplicate key update).

### Uitilities

- Database.get_table_names will give existing tables in the database
- Parameters such es `chunksize` for `pandas.to_sql` may be given to the wrapper function `send_data` and are handed over to pandas. The same is true for `send_query`.
- For transactions the context manager `transaction` may be of use.

## Installation

The package can be installed via pip:

```
pip install git+https://github.com/INWTlab/pydbtools.git
```

Publication on pypi will soon follow.
