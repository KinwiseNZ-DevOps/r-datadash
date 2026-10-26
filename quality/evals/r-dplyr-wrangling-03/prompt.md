---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): an RStudio Project for a public health class with a data/ folder, a tidyverse-based script, and a Shiny dashboard. The learner's data is a made-up outbreak line list. Answer from this description and the code below.

Code: cases |> group_by(sex, district) |> summarise(n = n()) then a later step mutate(share = n / sum(n)) gives shares that add to 1 within each sex, not overall. One district has no children so the table has a missing row.

Request: Why are the shares wrong and the row missing?
