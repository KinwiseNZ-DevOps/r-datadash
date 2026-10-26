---
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
---

Project context (nothing else is available to read): an RStudio Project for a public health class with a data/ folder, a tidyverse-based script, and a Shiny dashboard. The learner's data is a made-up outbreak line list. Answer from this description and the code below.

Code: cases |> filter(smoking_status == NA) returns 0 rows, but the learner can see blank smoking values in the table.

Request: Why does it return nothing and how do I fix it?
