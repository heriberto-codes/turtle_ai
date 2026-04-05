/backlog_update_prompt

## When to use
Use after the feature is complete to mark the corresponding backlog item as done.

## Inputs required
- completed feature name as written in backlog
- existing `docs/backlog.md`

## Output expected
- updated `docs/backlog.md`
- matching item changed from `- [ ]` to `- [x]`
- no other backlog items modified

Update docs/backlog.md to reflect completed work.

If docs/backlog.md does not exist, stop and report that the backlog file is missing.
Do not create a new backlog during BACKLOG UPDATE.

Task completed
[Feature name from backlog]

Rules
- Locate the matching backlog item.
- Change its status from - [ ] to - [x].
- Do not modify other items.
- Do not reorder sections.
- Do not remove completed items.
- Preserve formatting and headings.

Output the updated markdown for docs/backlog.md