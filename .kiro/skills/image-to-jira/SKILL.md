---
name: image-to-jira
description: Parse an image (screenshot, whiteboard photo, diagram, sticky-note board, roadmap, or planning doc) into structured Jira Epics and Stories for the ECM project. Use when the user attaches or references an image and wants tickets, epics, or stories drafted from it.
metadata:
  version: 1.1.0
---

## Purpose

Turn the contents of an image into an Epic/Story tree, then hand off to the Sprint
Ticket Agent for all field handling, preview, and creation. This skill owns only the
image-parsing step. It does NOT redefine field IDs, issue types, approval rules, or
creation mechanics — those live in the sprint agent prompt (`.kiro/agents/sprint-agent.md`)
and must not be duplicated here.

## What this skill does

1. **Read the image.** Describe what you see so the user can confirm the parse:
   headings, columns, groupings, arrows, and any text.
2. **Infer a ticket tree.**
   - Top-level groupings (columns, swimlanes, large boxes, headings, mind-map central
     nodes) become **Epics**.
   - Items nested under or connected to a grouping (cards, bullets, sticky notes, leaf
     nodes) become **Stories** under that Epic.
   - A flat list with no grouping becomes Stories only (no Epic), unless the user asks
     for an epic.
3. **Produce a draft tree** mapping each Epic to its child Stories, preserving on-image
   order.
4. **Hand off to the sprint agent.** From here, follow the Sprint Ticket Agent's
   existing rules for everything else — issue types, required fields (Epic Name, Epic
   Link, CSP, Description, Priority), the field-prompting flow, the full preview, waiting
   for explicit approval, and sequential creation. Do not restate those rules here.

## Parsing guidance

- When text is ambiguous or unreadable, show your best reading and ask the user to
  confirm or correct — do not guess silently.
- Preserve the on-image order of items.
- Drop decorative or stray content (logos, timestamps, unrelated scribbles, pasted URLs).
- If the image implies dependencies (arrows between items), mention them in the preview,
  but do not create issue links unless the user asks.
- One ticket per distinct unit of work — do not merge two cards into one Story.
