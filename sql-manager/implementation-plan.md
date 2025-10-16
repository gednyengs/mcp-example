# SQL Manager CLI Tool Implementation Plan

## Overview

Build a Python CLI tool using `argparse` and `sqlite3` (built-in) to manage a file-based SQLite database with Person entries.

## Database Schema

- **Table**: `Person`
- **Columns**: 
  - `id` (INTEGER PRIMARY KEY AUTOINCREMENT)
  - `name` (TEXT NOT NULL)
  - `lastname` (TEXT NOT NULL)
  - `age` (INTEGER NOT NULL)

## Project Structure

```
/Users/nyengele/temp/mcp-example/
├── sql_manager/
│   ├── __init__.py
│   ├── cli.py          # Main CLI entry point with argparse
│   ├── database.py     # Database operations class
│   └── models.py       # Person data model
├── setup.py            # Package installation config
├── requirements.txt    # Dependencies (minimal/none)
└── README.md          # Updated with usage instructions
```

## Command Interface

The tool will support these subcommands:

- `sql-manager create <db_file>` - Create new database
- `sql-manager add <db_file> --name <name> --lastname <lastname> --age <age>` - Add entry
- `sql-manager remove <db_file> --id <id>` - Remove entry by ID
- `sql-manager update <db_file> --id <id> [--name] [--lastname] [--age]` - Update entry
- `sql-manager get <db_file> --id <id>` - Retrieve single entry
- `sql-manager list <db_file>` - List all entries

## Implementation Details

### database.py

- `DatabaseManager` class with methods:
  - `create_database()` - Initialize DB and create Person table
  - `add_person()` - INSERT new person
  - `remove_person(id)` - DELETE by ID
  - `update_person(id, **kwargs)` - UPDATE fields by ID
  - `get_person(id)` - SELECT single person
  - `list_persons()` - SELECT all persons

### cli.py

- Use `argparse` with subparsers for each command
- Parse arguments and call appropriate DatabaseManager methods
- Format and display results (use simple print or tabulate for list)
- Handle errors gracefully with user-friendly messages

### setup.py

- Define entry point: `sql-manager = sql_manager.cli:main`
- Allows installation via `pip install -e .`

## Key Decisions

- Use built-in `sqlite3` library (no external dependencies)
- IDs are auto-generated for each person entry
- Database file path is required for each command (no default location)
- Simple text output formatting for results