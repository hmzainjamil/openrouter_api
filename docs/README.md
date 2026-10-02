# Supporting documentation

This directory contains SQL files and JSON audit artifacts. It is separate from the Rust API reference, which is generated from the crate source.

## Files

| Path | Role |
|---|---|
| [init_db.sql](./init_db.sql) | SQL schema/setup artifact |
| [insert_items.sql](./insert_items.sql) | SQL data insertion artifact |
| [agents/](./agents/) | Dated JSON audit and analysis artifacts |

The SQL files can change database state if executed. Read them and confirm the intended database and environment before running them. The JSON files are repository artifacts, not proof that the documented checks were executed against the current code.

For crate setup, API examples, build commands, security notes, and canonical source links, use the [root README](../README.md). For repository history and contribution workflow, see [CHANGELOG](../CHANGELOG.md) and [CONTRIBUTING](../CONTRIBUTING.md).
