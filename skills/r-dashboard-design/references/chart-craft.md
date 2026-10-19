# Polishing a single ggplot2 chart

Use this when a chart works but looks rough, or when a reader asks to make one chart clearer. It is about one plot; `house-style.md` is about the dashboard shell around it.

## Order of polish

Work in this order, checking the chart after each step rather than changing everything at once:

1. Get the right geometry and grouping first (the correct chart type, correct `fill` or `colour` grouping). Fix this before touching looks.
2. Apply one theme as a base (for example `theme_minimal()`), with a deliberate base size and font, before adding anything custom.
3. Add a title that states the point, not just the variable name, plus axis labels with units. Set `plot.title.position` and `plot.caption.position` to `"plot"` so text aligns with the whole panel, not just the axis.
4. Only then adjust colour, direct labels, and annotation.

## Reduce the need for a legend

- When there are only two or three groups, colour the group names directly inside the subtitle, axis text, or next to the data (using a markdown-capable text renderer such as `ggtext`, or plain text labels placed by hand) instead of a separate legend. The reader reads one less thing.
- Removing the legend and adding direct labels are one step, not two. A chart with the legend removed but no direct label anywhere is worse than the legend it replaced; do both in the same change, and check the rendered chart before calling it done.
- A direct label placed next to one group of points only covers the colours present at that spot. If the same colours also appear elsewhere in the chart (for example, in a different category), those appearances stay undecoded unless every colour used anywhere on the chart gets a label somewhere, even if that means labelling more than one spot.
- Order a categorical axis by a meaningful statistic, such as the size of the difference between its groups, rather than alphabetically. The sort order itself then carries information.
- Label a specific point or bar directly with a short, muted line and label instead of relying only on a legend to explain it. Give it enough vertical room that it does not sit on top of the data it names, and re-check the plot's width and title at the size it will actually be exported or displayed, not only on screen.

## Colour by data type

- Unordered categories: a qualitative palette (distinct hues, no implied order).
- A single value from low to high: a sequential palette, one hue getting lighter or darker.
- A value with a meaningful midpoint, such as zero or an average: a diverging palette, two hues either side of a neutral midpoint colour.
- To draw attention to one category without deleting the others, fade the rest instead of removing them, so the reader keeps the context.

## Finishing touches

- Remove dead space between a bar or column and its axis by setting the axis expansion to zero, so marks sit flush against the axis instead of floating.
- When exporting a series of charts that should look consistent, fix the width, height and resolution once and reuse it for every chart, rather than resizing each export by hand.

## Verify

Re-read the finished chart as a new reader would: can they state its point in ten seconds without a caption? If not, the labelling step was skipped, not the colour.

## Source

Principles were generalised from a worked example the maintainer supplied, showing one chart improved step by step in R with ggplot2, and confirmed by rebuilding the same steps against a different dataset. No original code or exact wording is reproduced here.
