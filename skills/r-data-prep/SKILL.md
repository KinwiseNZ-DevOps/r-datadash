---
name: r-data-prep
description: >
  Use when loading or cleaning data, calling an API with httr and jsonlite,
  scraping with rvest, or turning raw files into one tidy data frame. Governs
  getting data ready for a dashboard in R: reading CSV, Excel and JSON, calling
  web APIs, scraping tables, cleaning, date handling, and the extract-transform-
  load script shape. Do not use for database storage (r-sql-bridge), statistical
  tests (r-stats-foundations) or dashboard layout.
---

# R Data Prep

Stance: the extract, transform, load order and the check after each step are frozen; the packages (base R, tidyverse, data.table) are flexible and the project's style wins.

## Hard rules

- Check the HTTP status before parsing. Mechanism: symptom is a cryptic parse error; cause is an error page parsed as data; fix is to stop on any status other than 200 and show it.
- Do not hide missing data. Mechanism: symptom is a mean that looks fine; cause is rows silently dropped by `na.rm = TRUE`; fix is to count and report them. Reason: the dashboard will repeat whatever the data prep hid.
- No `setwd()` and no `rm(list = ls())` in scripts. Use an RStudio Project and `here::here()`.
- Keep keys, tokens and base URLs in one config place or environment variables, never pasted through the script.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Data is a clean file in the project | Read it and check it; no more |
| Data comes from an API | Check the status code before parsing |
| Data comes from a web page | Check the site's terms first; prefer an official download or API |
| Data holds personal information | Stop and ask what may be kept; do not scrape it |

## Steps

1. **Detect.** List the data files and scripts. Open the first rows (`head()`), then `str()` and `summary()`. Note missing values and the type of each column.
2. **Fix or build.** One script per stage, or one script with clear sections:
   - Extract: `read.csv()` or `readr::read_csv()`; for an API, `httr::GET(url)`, check `status_code == 200`, then `jsonlite::fromJSON(rawToChar(resp$content), flatten = TRUE)` or `httr::content(resp, "text")`.
   - Transform: fix types (`as.Date()`, `lubridate::year()`), derive columns, drop or fill missing values on purpose, and say which.
   - Load: write a tidy file (`readr::write_csv()`), or hand to [r-sql-bridge](../r-sql-bridge/SKILL.md).
   - Scraping: `rvest::read_html(url) |> html_element("table") |> html_table()`; if the first row is the header, move it into the names.
3. **Verify.** After each stage print row counts and `str()`. Compare one total with the source. Say what was not checked.

## References

Package docs: httr, jsonlite, rvest, readr, dplyr, lubridate on CRAN. Check the installed version for any argument you are unsure about.

## Next

- Store or share the data: [r-sql-bridge](../r-sql-bridge/SKILL.md)
- Analyse it: [r-stats-foundations](../r-stats-foundations/SKILL.md)
- Show it: [r-shiny-dashboards](../r-shiny-dashboards/SKILL.md), [r-quarto-dashboards](../r-quarto-dashboards/SKILL.md)
