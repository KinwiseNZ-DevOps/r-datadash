---
name: r-stats-foundations
description: >
  Use when computing a summary, running a z-test or t-test, fitting lm(), or
  explaining what a number means to a learner. Governs the statistics behind
  dashboards in R: mean, median, mode, variance, standard deviation, skewness,
  correlation, hypothesis tests and simple linear regression, and how to explain
  a result in plain words. Do not use for loading data (r-data-prep), chart
  choice (r-dashboard-design) or machine learning.
---

# R Stats Foundations

Stance: state the question, the method and the assumptions before the answer (frozen); which function or package runs it is flexible.

## Hard rules

- State hypotheses and assumptions before reporting a p-value. Mechanism: a p-value alone is misread; fix is a sentence on what it means. Reason: a learner remembers the verdict, not the caveat.
- Correlation is not causation, and a significant result is not a large or useful one. Report the size of the effect.
- Do not call a test "proved". Say "the data support" or "do not support".
- Use the learner's own data for examples, not made-up numbers, unless the example is labelled as made-up.
- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.

## Agent dispatch

Before handling a request, follow the agent selection in [r-datadash](../r-datadash/SKILL.md), including when this skill is invoked directly. Give the selected agent this skill's instructions. If no agent is available, continue yourself.

## Gate

| Situation | Action |
|---|---|
| Describe one variable | Mean, median, spread, skew; look at a plot too |
| Compare a sample with a known value | One-sample test; state the hypotheses |
| Two numeric variables | Correlation, then `lm()` if a line makes sense |
| Learner asks "does X cause Y?" | Explain that these tools show association, not cause |
| Tiny sample or broken assumptions | Say the result is weak; do not report a firm answer |

## Steps

1. **Detect.** Look at the data first: `summary()`, a histogram or box plot, and missing values. Note the sample size.
2. **Fix or build.**
   - Summaries: `mean()`, `median()`, `var()`, `sd()`, `cor()`. R has no built-in statistical mode; write a small function and name it `stat_mode`.
   - Teach the z-score by hand once: `(mean(x) - mu) / (sigma / sqrt(length(x)))`, saying where `sigma` comes from, then use `t.test()` or `prop.test()` for real work.
   - Regression: `model <- lm(y ~ x, data = df)`, `summary(model)`, plot with `ggplot(df, aes(x, y)) + geom_point() + geom_smooth(method = "lm", se = FALSE)`.
   - Write the result as a sentence: what was tested, the estimate, the p-value in words, and what it does not show.
3. **Verify.** Re-run with a fixed seed if random. Check one number by hand on a small sample. Say which assumptions were not checked.

## References

R documentation for `t.test`, `lm`, `cor`. Textbook: Introduction to Modern Statistics, https://openintro-ims.netlify.app/ . Check the installed R version for any argument you are unsure about.

## Next

- Show the result: [r-dashboard-design](../r-dashboard-design/SKILL.md)
- Data first: [r-data-prep](../r-data-prep/SKILL.md)
