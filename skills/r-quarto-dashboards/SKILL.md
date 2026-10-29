---
name: r-quarto-dashboards
description: >
  Use when writing or changing a .qmd dashboard, fixing its layout, or deciding
  between a static and an interactive version. Governs how to build and fix
  Quarto dashboards with R: the dashboard format, rows, columns, pages, tabsets,
  cards, sidebars, value boxes, and the optional Shiny server mode. Do not use
  for Shiny app structure (r-shiny-dashboards), data loading (r-data-prep) or
  chart choice (r-dashboard-design).
---

# R Quarto Dashboards

Stance: the heading-to-layout rules are frozen (they are Quarto's own); content and styling are flexible. Static output needs no server and is the default for teaching.

## Hard rules

- Replace placeholder cells (`plot(cars)`, empty chunks) before sharing. Mechanism: the page renders and looks finished; cause is starter code left in; fix is real data in every card. Reason: an empty card is a silent error.
- Heading text of rows and columns is discarded. Do not rely on it for labels; use `#| title:`. Reason: Quarto drops it, so the label never shows.
- Do not promise interactivity from a static render. Reason: inputs need `server: shiny` and a host that can run R.
- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Readers only look at numbers and charts | Static dashboard |
| Readers must change inputs and see new results | Add `server: shiny` (needs a running server) |
| The page is more of a report than a dashboard | Use a normal Quarto document |
| Unsure about an option | Check the Quarto docs for the installed version |

## Steps

1. **Detect.** Open the `.qmd`. Check the YAML for `format: dashboard`, the heading levels, and which cells have `#| title:`. Run `quarto --version`; dashboards need Quarto 1.4 or newer.
2. **Fix or build.**
   - Level 1 heading = page. Level 2 heading = row (or column). Each code cell under it becomes a card.
   - Switch direction with `format: dashboard: orientation: columns`, or per page with `# Name {orientation="columns"}`.
   - Size with `## Row {height=60%}` or `## Column {width=40%}`.
   - Tabs: `## Row {.tabset}` and `#| title: Name` in each cell.
   - Sidebar: a level 1 heading with `{.sidebar}`.
   - Every cell computes from real data loaded in the first chunk, and has a `#| title:`.
   - Value boxes: `#| content: valuebox` with a cell returning `list(icon=, color=, value=)`. `color: "light"` gives the calm, neutral tile that matches the house style in [r-dashboard-design](../r-dashboard-design/SKILL.md); a contextual colour such as `"primary"` fills the whole tile solidly instead. Check that the chosen icon (a Bootstrap Icons name) renders fully inside the tile; a wide or tall icon such as `"rulers"` can clip against the box edge, where a squarer icon such as `"arrows-angle-expand"` fits cleanly.
   - Interactive mode: `server: shiny`, expensive loading in a chunk with `#| context: setup`, reactive code in `#| context: server`, and `plotOutput()` with `renderPlot()`.
3. **Verify.** Run `quarto preview file.qmd` or `quarto render file.qmd`, open the result, and check each page. Say if rendering was not possible.

## References

Official docs: https://quarto.org/docs/dashboards/ , https://quarto.org/docs/dashboards/layout.html , https://quarto.org/docs/dashboards/interactivity/shiny-r.html . Fetched 18 October 2026.

## Next

- Data to show: [r-data-prep](../r-data-prep/SKILL.md)
- Chart and layout choices: [r-dashboard-design](../r-dashboard-design/SKILL.md)
- Full app instead: [r-shiny-dashboards](../r-shiny-dashboards/SKILL.md)
