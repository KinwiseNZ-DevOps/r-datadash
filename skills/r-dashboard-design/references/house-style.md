# House style for R dashboards

Use this when the user asks for a dashboard that looks modern and calm. It states the look; the tool skills state the syntax. Package arguments come from the bslib documentation (https://rstudio.github.io/bslib/); check the installed version before using one you are unsure about.

## The look

1. **Neutral base, one accent.** Greys for surfaces and text, one accent colour for the current page and the main action. Set `bg`, `fg` and `primary` in `bs_theme()` as hex codes or standard CSS colour names (for example `"#1a1a1a"` or `"white"`); an R graphics colour name such as `"grey10"` is not valid CSS and makes `bs_theme()` error out before the app starts. They change hundreds of defaults, so change little else.
2. **Key-number tiles.** A small muted title with a small change figure at its right (sign included, adverse change in red), a large bold value below, and any unit in smaller type beside the value. Use `value_box(title, value, showcase, ...)` and fill `value` from `textOutput()` so the layout does not jump.
3. **Quiet separation.** Light borders and small, even gaps. Avoid heavy shadows and thick outlines.
4. **Room to breathe.** Padding before decoration. One typeface, for example `font_google("Inter")`, in two or three sizes. A ggplot2 chart keeps its own font setting (`theme_minimal(base_family = ...)`); it does not pick up the app's `bs_theme()` font by itself, so if a chart should match an app-wide or user-chosen font, pass the same font name into `base_family` explicitly.
5. **Grid that adapts.** Tiles in `layout_column_wrap(width = "250px", fill = FALSE)` wrap from four across to one. Main charts in `layout_columns(col_widths = c(8, 4))`.
6. **Light and dark.** Use bslib's `input_dark_mode()` for the switch itself; it works and needs no extra wiring for the app's own Bootstrap chrome. Do not rely on `thematic::thematic_shiny()` alone to recolour a ggplot2 chart when the mode changes at runtime: tested with `thematic_shiny(session = session)` and the plot's render reading the mode input (so it does redraw), the chart still kept its light colours after switching to dark. The verified fix is to branch the plot's own colours on the mode input's value directly (`is_dark <- identical(input$<dark_mode_id>, "dark")`, then set `plot.background`, `panel.background`, `text` and `panel.grid` colours from that), inside the same `render*` that reads it. A user-facing accent-colour picker that calls `session$setCurrentTheme()` restyles the app's own controls (buttons, inputs, focus rings) correctly, but a `value_box` icon added with `shiny::icon()` does not follow it; recolour that icon from the same input if it must match.
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

- Open at laptop and phone width, and at one width in between (for example 800px). A sidebar plus wrapped tiles can clip a tile or force a horizontal scrollbar at a width neither "laptop" nor "phone" would catch.
- Switch to the dark theme and read every chart: confirm the chart panel itself changed colour, not just the page around it.
- Turn the accent colour to grey: does the page still make sense?
- If a tile or chart title is wrapped to a fixed character width, check it again in the narrowest view the app actually renders at (a `plotOutput` resizes with the browser; a fixed-width export does not). The same wrap width that fits a wide export can overflow a narrow panel.

## Source

Principles were read from a commercial UI kit the maintainer owns and from several dashboard template screenshots the maintainer supplied (a Tableau Accelerators template, an admin theme, a Coupler.io dashboard, and a KPI-design reference poster); no markup, branding, or image is reproduced here. See the maintainer's design notes, which are kept out of the public repo. The light/dark, accent-colour and font points were additionally verified by building and running a real bslib app with `input_dark_mode()`, a live accent-colour picker and a font picker, confirmed working and not working in each mode by hand.
