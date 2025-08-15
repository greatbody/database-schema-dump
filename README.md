# [🇨🇳 中文版 README](README_CN.md)
# database-schema-dump

Generates a single, re-runnable SQL script (`migration_script.sql`) that safely creates user-defined schemas, types, sequences, synonyms, and programmable objects (views, procedures, functions, triggers) from a supported database (SQL Server, Azure SQL, etc.).

## Supported Databases & Structure

- **Currently Supported:** Azure SQL Server (**Note:** This tool is more stable than `mssql-scripter`.)
- **Planned Support:** All major SQL databases (MySQL, PostgreSQL, Oracle, SQLite, etc.)

### New Structure

Each supported database type will have its own subfolder (e.g., `azure-sql-server/`, `mysql/`, `postgresql/`, etc.).

- Scripts and code for each database type are isolated in their respective subfolders.
- Only dependencies required for a specific database type are installed within its subfolder, reducing unnecessary package installations.
- This makes writing and running migration scripts simpler and avoids dependency conflicts.

## TODO

- [ ] Add subfolders for MySQL, PostgreSQL, Oracle, SQLite, etc.
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
1. Navigate to the subfolder for your target database (e.g., `cd azure-sql-server`).
2. Create and fill in `.env` based on `.env.example` (if present).
3. Install Python dependencies for that database type:

```bash
python -m pip install -r requirements.txt
```

## Run
```bash
python generate_migration.py
```
This will write `migration_script.sql` in the current subfolder.

## Notes
- Script orders objects as: schemas → types (alias, table) → sequences → synonyms → views/procs/functions/triggers to reduce dependency errors.
- For modules defined with ALTER, the script rewrites to CREATE.
- The output is intended to be re-run multiple times safely.
- Each subfolder is self-contained for its database type.

## Limitations
- Does not include tables, data, roles, permissions, or indexes (beyond PK definitions within table types when present).
- Complex dependency sorting among modules is minimal; the provided order generally works but may require manual tweaks in rare cases.
