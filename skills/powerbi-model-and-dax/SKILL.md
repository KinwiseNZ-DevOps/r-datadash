---
name: powerbi-model-and-dax
description: >
  Governs Power BI data models and DAX measures: star schema, relationships,
  date tables, storage modes, row-level security, and readable, fast DAX with
  variables and time intelligence. Use when designing or fixing a Power BI
  model, writing or debugging a DAX measure, or preparing database tables for
  Power BI from R. Do not use for report layout and colour (r-dashboard-design),
  database connections in R (r-sql-bridge) or R code.
---

# Power BI Model and DAX

Stance: a star schema with a proper date table is frozen; the storage mode and which measures exist are flexible. For product features that change often, check the current Microsoft Learn pages instead of trusting memory.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| One wide flat table | Split into facts and dimensions |
| Measure gives a wrong total | Debug filter context step by step |
| Model is slow or large | Remove unused columns, fix types, then measure |
| Data is live | DirectQuery or composite; check Microsoft docs for limits |
| Task is only report layout | Hand to [r-dashboard-design](../r-dashboard-design/SKILL.md) |

## Steps

1. **Detect.** List the tables and relationships (Model view, or ask the user to describe them). Identify facts (numbers and keys) and dimensions (descriptions). Find the date table.
2. **Fix or build.**
   - Model: one-to-many from dimension to fact, single-direction filters, no circular paths, a continuous date table, auto date/time off, foreign keys hidden. Many-to-many only through a bridge table.
   - Measures: use variables.
     ```dax
     Sales Growth % =
     VAR Current = [Total Sales]
     VAR Previous = CALCULATE ( [Total Sales], DATEADD ( 'Date'[Date], -1, YEAR ) )
     RETURN DIVIDE ( Current - Previous, Previous )
     ```
   - Write `Table[Column]` for columns and `[Measure]` for measures. Use `DIVIDE()` rather than `/`. Leave blanks blank.
   - Role-playing dates: one date table, extra relationships inactive, called with `USERELATIONSHIP`.
   - Row-level security: filter by user; test each role.
3. **Verify.** Check one measure against a hand-calculated total for one slice. Return each variable in turn to debug. Measure speed with Performance Analyzer. Say what was not checked.

## Hard rules

- Build a star schema before writing measures. Mechanism: symptom is a total that changes with the wrong slicer; cause is mixed grain or a flat table; fix is clean facts and dimensions.
- Prefer measures to calculated columns for numbers that aggregate. Reason: columns inflate the model.
- Do not use bidirectional relationships by default. Reason: they slow queries and create ambiguous filters.
- Never give security or licensing advice from memory; point to current Microsoft documentation.

## References

Condensed from the MIT-licensed awesome-copilot collection (Power BI data modeling and DAX agents). Official docs: https://learn.microsoft.com/power-bi/ and https://learn.microsoft.com/dax/ . Check them for current behaviour.

## Next

- Where the data comes from: [r-sql-bridge](../r-sql-bridge/SKILL.md)
- How the report looks and performs: [r-dashboard-design](../r-dashboard-design/SKILL.md)
