---
name: miro-change-map
description: Draw what an agent changed as a before-and-after map on a Miro board - which parts were added, removed or rewired, and what that did to the behaviour or the numbers. Use when the user asks "what did you change", "show me before and after", "what's different now", or has to understand a large agent-made change (a refactor, a migration, a new pipeline, a tuned config) without reading all of it.
---

# Map a change as before and after

An agent can change more in an hour than a person can read in a day. The question a human needs
answered is small: what did it look like before, what does it look like now, and what did that do?

## Find the change, don't guess it

1. Work from evidence: `git diff --stat <base>...HEAD`, the files you touched, the config or
   prompts you edited, the benchmark or test output before and after.
2. Name the unit that changed. It is usually a flow (how a request moves), a structure (which
   parts exist and who calls whom) or a setting (the defaults a system runs with).
3. Write the before in three to six steps and the after in three to six steps, in the same
   vocabulary, so the two rows can be compared box by box.
4. If a number moved (accuracy, latency, cost, error rate), put the before and after values in
   the last box of each row. Only use numbers you measured or can point to.

## Drawing

1. Call `canvas_get_canvas_composer_skill` first, then `canvas_load_format_skill` with
   `format_name: "diagramming"`, `notation: "flowchart"`.
2. Draw two separate Mermaid `flowchart LR` diagrams in one frame, "Before" on top and "After"
   below it. (Miro lays out one diagram with two subgraphs side by side, which gets too wide.)
3. Colors: added = green, removed or broken = orange, changed = blue, unchanged = no fill.
4. Put a bold "Before" and "After" label next to each diagram. Diagram titles only show on hover.
5. Title the frame with the question it answers ("What changed when it met real data?") and
   add the prompt as a caption.

## Miro gotchas

- Miro treats every diagram as a 1600x900 box when you resize a frame, even though it renders
  smaller. Leave the frame at least that tall below the lowest diagram's top edge.
- Leave about 64px above each diagram for its floating title chip.

## Finish

Share the frame link and name the one box a reviewer should check first, usually the most
important green or orange one.
