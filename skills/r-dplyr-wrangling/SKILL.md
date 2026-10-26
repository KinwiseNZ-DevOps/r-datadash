---
name: r-dplyr-wrangling
description: >
  Governs reshaping a data frame in R with dplyr before it feeds a dashboard:
  select and rename columns, filter rows (including missing values), make or
  recode columns with mutate and case_when, group and summarise, count, and
  work within groups. Use when writing or fixing dplyr code, when a filter
  returns zero rows or a recoded column has unexpected NA, when a grouped
  summary has missing groups or stays grouped, or when explaining these verbs
  to a learner. Do not use for reading files or APIs (r-data-prep), statistical
  tests (r-stats-foundations) or chart code (r-dashboard-design).
---

# R dplyr wrangling

Stance: check the data before and after every verb (frozen); base R, dplyr or data.table is flexible and the project's style wins. Teach one verb at a time, run it, look at the output, then combine.

## Hard rules

- Never filter with `== NA` or `== "NA"`. Mechanism: symptom is zero rows; cause is that a missing value is not equal to anything; fix is `is.na()`. Reason: the learner would conclude the data is empty.
- Never leave a `case_when()` without a default when unmatched rows matter. Mechanism: unmatched rows become `NA` and drop out of later counts. Fix: `.default = "Other"` or an explicit check.
- Never drop rows silently. Mechanism: `na.omit()` and `drop_na()` change the totals a dashboard shows; fix is to count the rows before and after and say so.
- Do not leave data grouped. Mechanism: a grouped table makes later `summarise()`, `mutate()` and `slice()` run per group and gives odd totals.
- Keep the original column until the new one is checked. Mechanism: overwriting loses the evidence needed to find a wrong rule.
- Use made-up or the learner's own data for examples, and label made-up data as such. Reason: real health records are personal.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Choose, drop, reorder or rename columns | `select()` or `rename()` |
| Keep or drop rows by a rule | `filter()`; decide what happens to `NA` first |
| New or recoded column | `mutate()`; `case_when()` for several rules, `if_else()` for two |
| Summary table or counts | `group_by()` then `summarise()`, or `count()`; then `ungroup()` |
| A value per row relative to its group | `group_by()` then `mutate()` or `arrange()`, then `ungroup()` |
| Reading files, APIs or dates | Hand to [r-data-prep](../r-data-prep/SKILL.md) |
| Data holds personal information | Stop and ask what may be kept; summarise before showing |

## Steps

1. **Detect.** Look at the columns and types (`glimpse()`), the first rows and the missing values. Name the question the table must answer.
2. **Build one verb at a time.** Run each step, print it, and check the row and column counts. Use [verb-patterns.md](references/verb-patterns.md) for the pattern and the trap of each verb.
3. **Handle `NA` on purpose.** Missing values never match `==` or `!=`. Use `is.na()` and decide in words whether missing rows are kept, dropped or labelled.
4. **Order the rules.** In `case_when()` the first matching rule wins, so write the most specific rule first and finish with an explicit `.default`. Check that no real value fell through to `NA`.
5. **Finish grouped work.** After `summarise()` on two or more grouping variables, or after any grouped `mutate()`, call `ungroup()` or set `.groups = "drop"`.
6. **Verify.** Compare one total with the source (the group counts add up to the row count), print the final shape, and say which checks were not made.

## Checklist

```text
- [ ] Types and missing values looked at before any verb
- [ ] Each verb run and its output looked at
- [ ] NA handled with is.na(), not == NA
- [ ] case_when rules ordered, with an explicit default
- [ ] Group counts add up to the row count
- [ ] Data ungrouped before further use
- [ ] Column names are snake_case with no spaces
```

## Example

Input: "My filter for adults returns nothing, and age_group is NA for some rows."
Output: the age column is text, or has `NA` values that `<` cannot compare. Convert it to numeric on purpose, add a rule or label for `NA`, put the rules in order from the most specific, and report how many rows ended up in each group so a missing group is visible.

## References

- [verb-patterns.md](references/verb-patterns.md): each verb's pattern, trap and a small made-up example.

Package docs: dplyr and tidyr on CRAN. Check the installed version for any argument you are unsure about, for example `.default` in `case_when()` needs dplyr 1.1.0 or later.

## Next

- Load and clean first: [r-data-prep](../r-data-prep/SKILL.md)
- Test or model it: [r-stats-foundations](../r-stats-foundations/SKILL.md)
- Show it: [r-dashboard-design](../r-dashboard-design/SKILL.md), [r-shiny-dashboards](../r-shiny-dashboards/SKILL.md), [r-quarto-dashboards](../r-quarto-dashboards/SKILL.md)
