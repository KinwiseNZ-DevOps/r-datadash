# dplyr verb patterns

Contents: select and rename, filter, mutate, case_when and if_else, summarise and group, count, grouped mutate and arrange, check list.

Distilled from the dplyr documentation and a public health data analysis course. All data below is made up. Each trap has its reason. The pipe shown is the native `|>`; `%>%` works the same way.

```r
cases <- tibble::tibble(
  id = 1:6,
  age = c(4, 17, 25, NA, 62, 40),
  sex = c("F", "M", "F", "M", "F", NA),
  district = c("North", "North", "South", "South", "South", "North")
)
```

## select and rename

- `select(cases, id, age)` keeps columns. `select(cases, !district)` drops one. `select(cases, id:age)` keeps a range. `select(cases, starts_with("a"))`, `ends_with()`, `contains()` and `everything()` help with many columns.
- `rename(cases, patient_id = id)` is `new = old`. Rename in one place, early, to snake_case.
- Trap: names with spaces or capitals need backticks later. Fix them once with `janitor::clean_names()` if the project already uses janitor.

## filter

- `filter(cases, age >= 18)` keeps rows where the rule is true. Several rules separated by commas mean "and". Use `|` for "or" and `%in%` for a list of values.
- Trap: `NA` is not equal to anything, so `filter(cases, age == NA)` returns no rows. Use `filter(cases, is.na(age))` to find them and `filter(cases, !is.na(age))` to drop them.
- Trap: `filter(cases, age >= 18)` also drops the row with `NA` age. If missing ages must stay, write `filter(cases, age >= 18 | is.na(age))`.
- Reason: a dashboard that quietly loses rows shows totals that nobody can reconcile.

## mutate

- `mutate(cases, age_months = age * 12)` adds a column; the same name overwrites. Several new columns can use each other in order.
- Convert types on purpose: `as.numeric()`, `as.Date()`, `factor()`. Check the result with `glimpse()`.
- A yes or no column: `mutate(cases, is_child = age < 18)`. It is `NA` where age is `NA`; say so.

## case_when and if_else

```r
cases |>
  mutate(age_group = case_when(
    is.na(age) ~ "Unknown",
    age < 18   ~ "Child",
    age < 65   ~ "Adult",
    .default   = "Older adult"
  ))
```

- The first matching rule wins, so each later rule can be loose. Putting `age < 65` first would label a child as an adult.
- Trap: a row that matches no rule becomes `NA`. Add `.default` (dplyr 1.1.0 or later; older code used `TRUE ~ "Other"`).
- Trap: all branches must give the same type (all text, all numbers).
- Trap: a comparison with `NA` gives `NA`, which never matches, so handle `is.na()` in its own rule, as above.
- `if_else(cond, "yes", "no")` is for two outcomes and is stricter about types than base `ifelse()`.

## summarise and group

```r
cases |>
  group_by(district) |>
  summarise(n = n(), mean_age = mean(age, na.rm = TRUE), n_missing_age = sum(is.na(age)))
```

- Always report how many were missing when using `na.rm = TRUE`.
- Trap: after grouping by two or more variables, `summarise()` drops only the last level and the result stays grouped. End with `ungroup()` or use `.groups = "drop"`.
- Trap: a group with no rows is absent from the result, so a dashboard table can show a gap or lose a category. To keep every combination, use `tidyr::complete()` after summarising, or `count(..., .drop = FALSE)` for factor columns.
- Check: the `n` values should add up to `nrow(cases)`.

## count

- `count(cases, district)` is shorthand for group, summarise `n()`, ungroup. Add `sort = TRUE` for largest first and `name = "cases"` to rename the count.
- `count(cases, district, sex)` counts each pair. `NA` is counted as its own group, which is useful for spotting missing data.

## grouped mutate, arrange and slice

- `group_by(district) |> mutate(share = n() / nrow(cases))` adds a value to every row from its group; the table keeps all rows. `ungroup()` after.
- `arrange(desc(age))` sorts; add `.by_group = TRUE` on a grouped table to sort inside each group first.
- `group_by(district) |> slice_max(age, n = 1)` gives the oldest case per district.
- Trap: running `mutate()` or `slice()` on a table that is still grouped from an earlier step gives per-group answers you did not intend. Check the header for `Groups:`.

## Check list for any wrangling script

1. `glimpse()` the input.
2. Print the row count after each filter.
3. After recoding, run `count()` on the new column and check no unexpected `NA`.
4. After summarising, compare the counts with the source.
5. Save the tidy result to a file the dashboard reads, and keep the script in the project.

Related: [r-data-prep](../../r-data-prep/SKILL.md) for reading data, [r-stats-foundations](../../r-stats-foundations/SKILL.md) for tests.
