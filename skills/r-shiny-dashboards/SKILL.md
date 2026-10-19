---
name: r-shiny-dashboards
description: >
  Governs how to build and fix Shiny and bs4Dash dashboards in R: file layout,
  global setup, ui and server split, reactivity, value boxes, tabs, charts and
  static assets. Use when writing or changing a Shiny app, a bs4Dash page, a
  value box, a plotly or leaflet output, or the www folder. Do not use for
  Quarto dashboards (r-quarto-dashboards), data loading (r-data-prep) or chart
  and layout choices (r-dashboard-design).
---

# R Shiny Dashboards

Stance: the split between data, ui and server is frozen; the packages (plain Shiny, bs4Dash, shinydashboard, bslib) are flexible and the project's existing choice wins.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Existing app runs and the task is small | Change only the named piece |
| New dashboard from scratch | Create the file layout below first |
| A tile or chart shows a fixed or fake number | Treat as a defect and replace it with a computed value |
| New dashboard that should look modern | Prefer bslib (`page_sidebar`, `layout_columns`, `value_box`); see the house style in [r-dashboard-design](../r-dashboard-design/SKILL.md) |
| The app only needs to be read, not clicked | Suggest [r-quarto-dashboards](../r-quarto-dashboards/SKILL.md) |

## Steps

1. **Detect.** Find `app.R` or `ui.R` and `server.R`, `global.R`, the `www/` folder, and the data files. Note which layout package is loaded (`library(bs4Dash)` or another).
2. **Fix or build.**
   - One place for packages and shared data: `global.R` (or the top of `app.R`). Load data once there, not inside `render*` calls.
   - Split large UIs into section files (`header.R`, `sidebar.R`, `body.R`, `footer.R`) and `source()` them with `local = TRUE` paths relative to the app folder.
   - Menu items and `tabItem` names must match exactly (`menuItem(tabName = "sales")` and `tabItem(tabName = "sales")`).
   - Values come from data: `renderValueBox({ valueBox(format(total, big.mark = ","), "Total sales", icon = icon("coins")) })`.
   - Charts react to inputs: `plotlyOutput("trend")` with `output$trend <- renderPlotly({ ... })`. Use `reactive()` for a filtered data set that several outputs share.
   - Put images and CSS in `www/` and refer to them without the `www/` prefix. Compile SCSS with the `sass` package; `includeCSS()` only accepts plain CSS.
3. **Verify.** Run the app (`shiny::runApp()`), click every menu item and input, and compare one tile with a number computed by hand in the console.

## Hard rules

- Do not read files or query a database inside a `render*` call on every redraw. Mechanism: the app slows with each click; cause is repeated work; fix is to load once in `global.R` or in a `reactive()`.
- Do not export a chart to HTML and embed it in an iframe when an input should change it. Mechanism: the chart stays frozen; fix is `renderPlotly` or `renderPlot`. Reason: interactivity is the point of Shiny.
- No `setwd()`; use relative paths from the app folder. Reason: the app must run on any machine.
- Never put a password or API key in `ui.R`, `server.R` or `global.R`. Use environment variables ([r-sql-bridge](../r-sql-bridge/SKILL.md)).

## References

Official docs: https://shiny.posit.co/r/ and https://rinterface.github.io/bs4Dash/ . Check the installed package version before using an argument you are unsure about.

## Next

- Data to show: [r-data-prep](../r-data-prep/SKILL.md)
- Chart and layout choices: [r-dashboard-design](../r-dashboard-design/SKILL.md)
