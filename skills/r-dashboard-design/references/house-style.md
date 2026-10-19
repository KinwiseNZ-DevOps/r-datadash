# House style for R dashboards

Use this when the user asks for a dashboard that looks modern and calm. It states the look; the tool skills state the syntax. Package arguments come from the bslib documentation (https://rstudio.github.io/bslib/); check the installed version before using one you are unsure about.

## The look

1. **Neutral base, one accent.** Greys for surfaces and text, one accent colour for the current page and the main action. Set `bg`, `fg` and `primary` in `bs_theme()`; they change hundreds of defaults, so change little else.
2. **Key-number tiles.** A small muted title with a small change figure at its right (sign included, adverse change in red), a large bold value below, and any unit in smaller type beside the value. Use `value_box(title, value, showcase, ...)` and fill `value` from `textOutput()` so the layout does not jump.
3. **Quiet separation.** Light borders and small, even gaps. Avoid heavy shadows and thick outlines.
4. **Room to breathe.** Padding before decoration. One typeface, for example `font_google("Inter")`, in two or three sizes.
5. **Grid that adapts.** Tiles in `layout_column_wrap(width = "250px", fill = FALSE)` wrap from four across to one. Main charts in `layout_columns(col_widths = c(8, 4))`.
6. **Light and dark.** Define both themes up front and switch with `session$setCurrentTheme()`. Use `thematic::thematic_shiny()` so plots follow the theme.
7. **The right shell.** `page_sidebar()` when filters apply to everything; `page_navbar()` with `nav_panel()` when there are a few separate views.
8. **Change is never colour alone.** Pair red or green with a sign, an arrow or a word.

## KPI tile variants

Beyond the single tile style in point 2 above, choose by what the reader needs to see at a glance. Pick one style per dashboard and keep it for every tile on the page; mixing styles tile to tile reads as unfinished, not varied.

- **Plain number.** Big value only, no decoration. Fine when there are many tiles in one filter bar and space is tight.
- **Trend fill.** A thin sparkline or soft area behind or below the value, no axis. Shows shape, not detail.
- **Progress to target.** A single bar split into reached and remaining, with the percentage printed. Use when a target matters more than the trend.
- **Reference tick.** A bar for the current value with a short tick mark at the reference value, so the reader compares two numbers in one shape instead of two bars.
- **Category breakdown.** Several short horizontal bars under one tile, one per category, each with its own value. Use only when the categories are the point, not a distraction from the headline number.

## Ranked lists and maps

- A ranked list, for example top sites or top pages, reads fastest as a numbered row with an inline horizontal bar and the value beside it, sorted highest first. Stop at six to eight rows; add a "see all" link rather than scrolling.
- A map earns its place only when location is the point. Size a bubble by volume rather than colouring a filled region, unless the regions are few and the differences are large; many thin colour shades are harder to compare than circle sizes.

## Avoid

- Do not place chat, social feed, or user-management widgets on the same page as a KPI dashboard. Reason: they compete for attention with the numbers the page exists to show, and they break the one-question-per-page rule in the parent skill.

## Choosing bslib or bs4Dash

Prefer bslib for a new dashboard. Keep bs4Dash when the project already uses it and the task is small. Do not mix them in one app. Pin `bs_theme(version = 5)` for bslib in anything shared.

## Checks

- Open at laptop and phone width.
- Switch to the dark theme and read every chart.
- Turn the accent colour to grey: does the page still make sense?

## Source

Principles were read from a commercial UI kit the maintainer owns and from several dashboard template screenshots the maintainer supplied (a Tableau Accelerators template, an admin theme, a Coupler.io dashboard, and a KPI-design reference poster); no markup, branding, or image is reproduced here. See the maintainer's design notes, which are kept out of the public repo.
