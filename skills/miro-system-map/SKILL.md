---
name: miro-system-map
description: Read the current repository and draw how the system is put together today as a block diagram on a Miro board - runtimes, services, data stores, external APIs and the arrows that write data. Use when the user asks to "draw the current setup", "diagram the architecture", "show me how this repo fits together", or is onboarding onto an unfamiliar or agent-written codebase.
---

# Draw the current system as a block diagram

The goal is the picture a new engineer would draw after a week, produced from the code in ten
minutes. Draw what the code does today, not what the README says it should do.

## Gather facts from the code

1. Entry points: framework routes, API handlers, CLIs, workers, cron jobs.
2. Data: ORM schemas, migrations, connection env vars (names only, never values).
3. Outbound calls: SDK imports and `fetch` targets (email, payments, analytics, LLM providers).
4. Deployment: `vercel.json`/`vercel.ts`, Dockerfiles, IaC, CI workflows.
5. For every arrow you plan to draw, find the line of code that makes the call. If you can't find
   it, leave the arrow out.

## Drawing

1. Call `canvas_get_canvas_composer_skill` first, then `canvas_load_format_skill` with
   `format_name: "diagramming"`, `notation: "flowchart"`.
2. One Mermaid `flowchart LR`:
   - a `subgraph` per place the code runs (for example "Vercel: Next.js", "Worker", "Browser")
   - data stores as cylinders `[(...)]`
   - third-party services in one neutral color, your code in one color, data in a third
   - who calls whom as arrows. Label every arrow that writes or sends data
     ("writes orders", "sends receipts"); leave reads unlabeled
3. 10-15 nodes. If it needs more, draw a top-level map plus one frame per subsystem.
4. Title the frame with a question the diagram answers ("What is <project>, today?").
5. Add the prompt as a caption under the diagram.

## Miro gotchas

- Quote labels that start with `/` or contain brackets: `blog["/blog/[slug]"]`.
- Diagram widgets size themselves, and the canvas tools can't move them after creation. Size the
  frame to the rendered diagram, not to what you authored.
- Leave about 64px above the diagram for its floating title chip.

## Keep it private

A system map lists every endpoint, auth path and data store in one picture. That's the first page of a
threat model. Draw it on a board only your team can open, and never paste it into public docs or posts.

## Finish

Share the frame link and list anything surprising the map exposed, such as a service two
subsystems both write to, or a dependency no one documented.
