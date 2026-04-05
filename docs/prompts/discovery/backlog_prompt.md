/backlog_prompt

## When to use
Use after IDEATE to convert ideas into a persistent backlog, or when updating the backlog with new items.

## Inputs required
- IDEATE output
- existing `docs/backlog.md` if present

## Output expected
- updated `docs/backlog.md`
- markdown task list
- grouped sections
- preserved status on existing items
- only new items appended when needed

Convert the IDEATE results into a structured product backlog.

Group items into:
- Features
- Architecture Improvements
- Client Improvements

Formatting rules

Use markdown task list format for every item:

- [ ] Not started
- [x] Completed

Preserve the status of existing items when updating the backlog.

If an item already exists in docs/backlog.md:
- keep its existing status
- do not duplicate it

If the item is new:
- add it as unchecked (- [ ])

Optional:
- Within each section, list the most practical / highest-leverage items first

Output the backlog as markdown.

Save to docs/backlog.md.
If docs/backlog.md does not exist, create it.
If docs/ does not exist, create it.

Do not remove completed items.
Do not change checked items back to unchecked.
Only append new items if they do not already exist.