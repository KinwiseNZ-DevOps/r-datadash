---
name: r-datadash
description: >
  Router and shared guardrails for building, explaining and checking data
  dashboards with R, plus the SQL and Power BI links around them, aimed at
  students and lecturers. Use when a task touches an R dashboard (Shiny,
  bs4Dash, Quarto), data preparation for one, a database link, Power BI, or
  when it is unclear which specialist skill applies. Do not use for Python
  dashboards or for non-dashboard R work such as package development.
---

# R DataDash (router)

Stance: the habits are frozen (every number traces back to data the learner can inspect, nothing depends on one person's computer, the agent explains why), the tool choices inside them are flexible. This skill routes to the right specialist and enforces the guardrails every change must satisfy.

## Gate

| Situation | Action |
|---|---|
| Building or fixing a Shiny or bs4Dash app | [r-shiny-dashboards](../r-shiny-dashboards/SKILL.md) |
| Building or fixing a Quarto dashboard | [r-quarto-dashboards](../r-quarto-dashboards/SKILL.md) |
| Loading, cleaning, scraping or calling an API for data | [r-data-prep](../r-data-prep/SKILL.md) |
| Connecting R to a database or handing data to Power BI | [r-sql-bridge](../r-sql-bridge/SKILL.md) |
| Statistics, tests, regression, explaining a result | [r-stats-foundations](../r-stats-foundations/SKILL.md) |
| Which chart, layout, colours, speed | [r-dashboard-design](../r-dashboard-design/SKILL.md) |
| Power BI model, relationships, DAX | [powerbi-model-and-dax](../powerbi-model-and-dax/SKILL.md) |
| Task spans several rows above | Start at r-data-prep, then r-stats-foundations, then the dashboard tool skill, then r-dashboard-design |
| Python, Excel-only or Tableau work | Say it is out of scope; offer the nearest R approach |

## Agent selection

When this skill or a sibling is invoked, use the `r-datadash-workflow` profile when available. It handles normal requests and has plan-only, debug, and implement or review modes. For a reported defect or a wrong number it may use the optional `r-datadash-debugger` helper. Give the agent the user's request and the relevant specialist skill, then review its work against that skill's hard rules. If no profile or delegation is available, carry out the same workflow directly. Do not stop to ask the user to choose an agent.

## Steps

1. **Detect.** Open the project. Find the entry point (`app.R`, `ui.R` and `server.R`, or a `.qmd`), the data files, and whether it is an RStudio project. Read its `README` and any `renv.lock`.
2. **Route.** Pick the specialist from the gate table and follow it. For multi-part work, go in dependency order: data, statistics, dashboard, design.
3. **Explain.** Say in plain words what changed and why. A learner should be able to repeat the reason.
4. **Verify.** Run the dashboard or render the document and look at the result. Run the project's tests if any. Say what was checked and what was not.

## Hard rules

- Every number on screen comes from data the learner can inspect. Mechanism: symptom is a tile that never changes; cause is a placeholder left in (`10*2`, `plot(cars)`); fix is to compute it from the loaded data and show where. Reason: a dashboard that looks right but is not tied to data teaches the wrong habit.
- No `setwd()` with a personal path, no passwords in code, no `rm(list = ls())`. Mechanism: the script runs on one computer only, or wipes the next person's session. Fix: an RStudio Project, `here::here()`, and environment variables. Reason: classroom code must run on every student's machine.
- Never invent package names, functions or options. Check the installed version or the official docs when unsure. Reason: a fake function costs a learner an hour.
- Make the smallest correct change and keep the project's own style. Reason: a student must be able to read the diff.

## Next

All specialist skills are linked in the gate table above.
