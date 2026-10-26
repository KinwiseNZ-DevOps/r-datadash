---
name: r-dashboard-design
description: >
  Use when deciding which chart to use, laying out a page, fixing an unreadable
  or slow dashboard, or reviewing one for clarity. Governs how a dashboard looks
  and performs: choosing a chart, arranging the page, colour and accessibility,
  labelling, and speed habits for R and Power BI dashboards. Do not use for tool
  syntax (r-shiny-dashboards, r-quarto-dashboards, powerbi-model-and-dax) or for
  the statistics itself (r-stats-foundations).
---

# R Dashboard Design

Stance: the reader's question decides the chart and the layout (frozen); the colours, fonts and tool are flexible.

## Hard rules

- Measure before tuning. Mechanism: guessing speeds up the wrong part; fix is a timing or Performance Analyzer reading before and after. Reason: unmeasured tuning adds complexity for no gain.
- Do not truncate an axis to exaggerate a change. Reason: it misleads the reader.
- Treat the quoted targets (load under ten seconds, interactions under three) as rules of thumb, not guarantees.
- Never use colour alone to carry meaning. Reason: one in twelve men has a colour-vision difference.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Reader wants to compare categories | Bar or column chart |
| Reader wants a trend | Line chart |
| Reader wants a relationship | Scatter plot |
| Reader wants a spread | Histogram or box plot |
| Many parts of a whole | Stacked or sorted bars; not a pie |
| Page already has too many visuals | Split pages or add drill-through |
| User wants a modern, calm look | Apply [house-style.md](references/house-style.md) |
| One chart looks rough or needs polishing | Apply [chart-craft.md](references/chart-craft.md) |

## Steps

1. **Detect.** Ask or infer the reader and the single question the page answers. Count the visuals and list the colours used.
2. **Fix or build.**
   - Put the most important item top-left; a row of 3 to 5 key numbers across the top; one or two main charts; filters at the left or top.
   - Few visuals per page. About six to eight is a guide, not a rule.
   - Every chart has a title that states the point, labelled axes with units, and a "last updated" note.
   - Colour: do not use colour as the only cue; keep text contrast at least 4.5 to 1; use a colour-blind-safe palette; use red and green only with a second cue such as an arrow or label.
   - Speed in R: load data once, share filtered data with `reactive()`, aggregate before plotting, cache slow steps.
   - Speed in Power BI: measure with Performance Analyzer first, then cut visuals, shrink the model and simplify measures.
3. **Verify.** Look at the page at laptop and phone width. Ask: can a new reader state the main message in ten seconds? Say what was not tested.

## References

Look and feel: [house-style.md](references/house-style.md) (load when the user wants a modern, calm style). Source ideas are condensed from the MIT-licensed awesome-copilot collection (Power BI visualization and performance agents), generalised for R. Single-chart polish: [chart-craft.md](references/chart-craft.md) (load when one chart needs improving). Accessibility: https://www.w3.org/WAI/WCAG22/quickref/ .

## Next

- Build it: [r-shiny-dashboards](../r-shiny-dashboards/SKILL.md), [r-quarto-dashboards](../r-quarto-dashboards/SKILL.md), [powerbi-model-and-dax](../powerbi-model-and-dax/SKILL.md)
