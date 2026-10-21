---
inclusion: always
---

# Project Memory

_Last distilled: (not yet distilled)_

> The session-memory-agent fills in and maintains the sections below over time.
> Keep this file under 100 lines. Do not store secrets or volatile status (ticket state lives in Jira).

## Key Decisions
<!-- Lasting decisions and their rationale. Include ticket IDs verbatim. -->

## Jira & Project Context
<!-- Durable project facts: field mappings, project keys, conventions.
     Example field mappings for a Jira Data Center setup:
     - Epic Link field = customfield_10000
     - Epic Name field = customfield_10002
     - Issue type IDs: Epic = 10000, Story = 10001 -->

## Architecture & Patterns
<!-- Reusable technical facts and API patterns confirmed to work. -->

## Recall Protocol

If the user asks a question about past work, decisions, or conversations and the answer is not in this file:
1. Check this file (MEMORY.md) first. If the answer is here, answer directly — do NOT invoke the session-memory-agent.
2. Only invoke session-memory-agent if the answer is NOT in this file.
3. Use specific search terms from the topic being asked about.

Knowledge base details:
- SessionHistory KB is indexed from your personal sessions folder (see session-memory-agent.json).
- Use specific terms from the topic being asked about. Generic queries may return nothing — retry with more targeted terms if the first attempt fails.

## Corrections & Notes
<!-- Known gotchas and blockers worth remembering. -->
