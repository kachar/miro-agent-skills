---
name: miro-decision-chart
description: Turn the numbers behind a hard technical decision into a chart on a Miro board, with the chosen option highlighted and the decision written underneath. Use when the user asks to "chart these results", "help me decide between", "visualize this benchmark/comparison", or needs to explain a trade-off (accuracy vs cost vs latency) to people who won't read the raw table.
---

# Chart a decision

A decision chart shows the options, the one you picked, and one sentence explaining why, in that
order. It is not a dashboard.

## Get the numbers right first

1. Take numbers only from a source you can point to: a benchmark output file, a query result, a
   design doc, a spreadsheet. Record where each one came from.
2. Choose one primary metric for the bars (accuracy, p50 latency, monthly cost). Secondary
   metrics go in a text label next to each bar, not in extra bars.
3. Sort options by the primary metric. Keep ranges as ranges ("38-40%"), not midpoints.

## Drawing

Miro has no native chart widget, so build a horizontal bar chart from shapes. The canvas SVG
format supports it well:

1. Call `canvas_get_canvas_composer_skill` first and use the Bright Paper palette it returns.
2. One frame. Title = the decision as a question ("Which tool search ships by default?").
   A one-line subtitle states the metric, the dataset and the unit.
3. Per option: a right-aligned `textArea` label, a `rect` bar whose width is proportional to the
   value (fix one scale, such as 14px per percentage point, and use it for every bar), and a
   `textArea` after the bar with the value and the secondary metrics separated by " / ".
4. Every bar in one neutral color, the chosen option in green with a bold label.
5. Under the bars, a light green rounded rect with the decision and the one reason that settled
   it ("four points behind the best at a quarter of the cost").
6. Caption with the source of the numbers.

## Finish

Share the frame link and state which number would change the decision if it moved, so the chart
can be redrawn when that number is re-measured.
