---
name: r-sql-bridge
description: >
  Use when writing dbConnect code, moving a data frame into a database, querying
  from a dashboard, or feeding Power BI from a database. Governs connecting R to
  a SQL database and handing data on to Power BI: DBI connections, safe
  credentials, reading and writing tables, parameterised queries, and choosing
  SQLite or PostgreSQL for teaching. Do not use for cleaning data (r-data-prep)
  or Power BI modelling and DAX (powerbi-model-and-dax).
---

# R SQL Bridge

Stance: the connect, use, disconnect order and credentials outside code are frozen; the database engine and driver are flexible. SQLite needs no server and suits classrooms; PostgreSQL suits larger shared work.

## Hard rules

- No password, token or `root` login in code. Mechanism: symptom is a secret in git history; cause is a literal in a script; fix is `Sys.getenv()` and a limited user. Reason: pushed secrets cannot be recalled.
- Parameterise queries. Mechanism: SQL injection through pasted text; fix is `params`.
- `overwrite = TRUE` and `DROP`/`DELETE` need explicit confirmation. Reason: they destroy data that may have no backup.
- Check the current CRAN status of a driver before recommending it; older drivers such as RMySQL are not recommended.
- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Learner has no database server | Use SQLite (`RSQLite`) |
| Shared or larger data | PostgreSQL (`RPostgres`) or MariaDB/MySQL (`RMariaDB`) |
| Task would delete or overwrite a table | Show the effect and ask first |
| The user gives a password in chat | Do not put it in code; show the environment variable route |

## Steps

1. **Detect.** Find existing `dbConnect` calls, `.Renviron`, and any `.sql` files. Inspect the live database with `DBI::dbListTables()` and `dbListFields()` rather than guessing the structure from code.
2. **Fix or build.**
   - Connect with credentials read from environment variables: `DBI::dbConnect(RPostgres::Postgres(), dbname = Sys.getenv("DB_NAME"), host = Sys.getenv("DB_HOST"), user = Sys.getenv("DB_USER"), password = Sys.getenv("DB_PASSWORD"))`. Keep the values in `.Renviron`, which is git-ignored.
   - Close the connection even on error: `on.exit(DBI::dbDisconnect(con), add = TRUE)` inside functions.
   - Read with `dbGetQuery(con, "SELECT ... WHERE x = $1", params = list(value))`. Never paste user input into the SQL string.
   - Write with `dbWriteTable(con, "name", df, append = TRUE)`. Use `overwrite = TRUE` only on purpose, after saying which table it replaces.
   - Power BI hand-off: tell the learner to create a read-only database user, then connect Power BI to the tables with its database connector, and to model them as a star schema ([powerbi-model-and-dax](../powerbi-model-and-dax/SKILL.md)).
3. **Verify.** Count rows before and after a write. Run one query and compare a total with the data frame. Say if the database could not be reached.

## References

Packages: DBI, RSQLite, RPostgres, RMariaDB. Docs: https://dbi.r-dbi.org/ . PostgreSQL manual: https://www.postgresql.org/docs/ .

## Next

- Prepared data to load: [r-data-prep](../r-data-prep/SKILL.md)
- Power BI side: [powerbi-model-and-dax](../powerbi-model-and-dax/SKILL.md)
