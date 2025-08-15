# database-schema-dump

Generates a single, re-runnable SQL script (`migration_script.sql`) that safely creates user-defined schemas, types, sequences, synonyms, and programmable objects (views, procedures, functions, triggers) from a SQL Server or Azure SQL database.


## Supported Databases

- **Currently Supported:** Azure SQL Server (**Note:** This tool is more stable than `mssql-scripter`.)
- **Planned Support:** All major SQL databases (MySQL, PostgreSQL, Oracle, SQLite, etc.)

## TODO

- [ ] Add support for MySQL
- [ ] Add support for PostgreSQL
- [ ] Add support for Oracle
- [ ] Add support for SQLite
- [ ] Improve migration script generation
- [ ] Add automated tests
- [ ] Enhance documentation
## Prerequisites
- Python 3.9+
## Setup
1. Create and fill in `.env` based on `.env.example`.
2. Install Python deps:

```bash
python -m pip install -r requirements.txt
```

## Run
```bash
python generate_migration.py
```
This will write `migration_script.sql` in the current directory.

## Notes
- Script orders objects as: schemas → types (alias, table) → sequences → synonyms → views/procs/functions/triggers to reduce dependency errors.
- For modules defined with ALTER, the script rewrites to CREATE.
- The output is intended to be re-run multiple times safely.

## Limitations
- Does not include tables, data, roles, permissions, or indexes (beyond PK definitions within table types when present).
- Complex dependency sorting among modules is minimal; the provided order generally works but may require manual tweaks in rare cases.
