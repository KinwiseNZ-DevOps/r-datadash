---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): an RStudio Project for a public health class with a data/ folder, a tidyverse-based script, and a Shiny dashboard. The learner's data is a made-up outbreak line list. Answer from this description and the code below.

Code: cases |> mutate(age_group = case_when(age < 120 ~ 'Older adult', age < 30 ~ 'Young adult', age < 18 ~ 'Child')). Every person is labelled Older adult, and two people with a missing age show NA.

Request: Fix the age groups.
