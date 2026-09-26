---
name: miro-flow-trace
description: Trace one request, job or user action through the code and draw it as a UML sequence diagram on a Miro board, including auth branches, retries and error paths. Use when the user asks "what happens when...", "trace this flow", "how does X get from A to B", or needs to review a change to a request path they didn't write.
---

# Trace one flow as a sequence diagram

A sequence diagram answers one question: in what order do these parts talk, and what happens when
something fails? Pick exactly one flow per diagram.

## Trace it in the code

1. Start at the entry point the user named (route, handler, queue consumer, CLI command).
2. Follow the calls in order. For each hop write down the caller, the callee, the message (the
   function or HTTP call) and the file it happens in.
3. Record the branches that matter: auth paths, cache hit or miss, retries, the error the
   caller actually sees. Skip logging and metrics.
4. Participants are components (route handler, auth library, DB), not individual functions.
   Five to seven participants is plenty.

## Drawing

1. Call `canvas_get_canvas_composer_skill` first, then `canvas_load_format_skill` with
   `format_name: "diagramming"`, `notation: "uml_sequence"`.
2. One Mermaid `sequenceDiagram`. Use `alt` / `else` for branches and a nested `alt` for the
   failure case inside a branch. Dashed arrows (`--&gt;&gt;`) for responses. Remember that the
   body is XML, so `-&gt;&gt;` not `->>`.
3. Message labels are the real call names from the code (`verifySession`, `POST /orders`),
   so a reviewer can grep for them.
4. Sequence diagrams render tall. Give the frame at least 1,300px of height and check the
   rendered size before placing the caption.

## Keep it private

Auth flows are the part of a system an attacker most wants mapped. Keep these frames on a team-only board,
and leave secrets out of the labels: name an env var if you must, never its value.

## Finish

Share the frame link and name the one step where you'd put a breakpoint or a test if this flow
broke in production.
