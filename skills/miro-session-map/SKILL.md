---
name: miro-session-map
description: Draw what the current agent session did, or is about to do, as a flowchart on a Miro board - steps, parallel branches, decision gates and where it ended. Use when the user says "map this session", "draw what we just did", "document this process on the board", or wants a visual record of a long agent run before reviewing its output.
---

# Map this session on a Miro board

A long agent session is a process nobody watched end to end. This skill turns it into one
flowchart a human can check in thirty seconds.

## Before drawing

1. Ask for the board URL if you don't have one. If the user gave a frame link
   (`?moveToWidget=<id>`), read that frame first with `canvas_read_as_svg` and draw inside it.
2. Reconstruct the session from evidence, not memory: the conversation, `git log --oneline`,
   `git diff --stat`, files you created, subagents you started, tests you ran. Every node must map
   to something that happened (or, for a plan, something you will do next).
3. Keep it to 8-15 nodes. Merge small steps. If the session had more than 15 real steps,
   draw one frame per phase instead of one crowded diagram.

## Drawing

1. Call `canvas_get_canvas_composer_skill` first (it is required), then
   `canvas_load_format_skill` with `format_name: "diagramming"` and `notation: "flowchart"`.
2. One Mermaid `flowchart LR` inside a `<foreignObject data-type="diagram">`:
   - start and end nodes as stadiums `([...])`
   - work as rectangles, decision gates as `{{...}}` with labeled `Yes` / `No` edges
   - parallel work (subagents, background jobs) as sibling branches from the same node
   - loops that actually happened (a failed test, a re-fetch) as edges back to the gate
3. Label nodes with the concrete thing, not the category: "Subagent: benchmark research", not
   "Research".
4. Under the diagram, add one line with the prompt that produced it, so the board explains itself.

## Miro gotchas

- A diagram widget sizes itself from the Mermaid source and cannot be moved or deleted with the
  canvas tools after creation. Choose its position first, then fit the frame around the rendered
  size.
- Its title chip floats just above it. Leave about 64px of empty space above the diagram.
- Children of a frame use frame-relative coordinates.
- Quote any label that starts with `/` (`a["/api/orders"]`) or Mermaid draws a parallelogram.

## Finish

Report the frame link (`<board>?moveToWidget=<frame id>`) and one sentence on what the map shows
that the transcript hides, such as a gate that failed twice.
