# Git Finalization Skills

## Summary

Added `turtle-add` and `turtle-push`, and changed `turtle-commit` to create and verify one commit from reviewed staged changes. The sequence is `$turtle-add` -> `$turtle-commit` -> `$turtle-push`, with each skill invoked separately after the existing workflow gates.

## Architecture Impact

- Turtle now has 24 Markdown skill contracts. No application runtime, APIs, dependencies, or additional state files were introduced.
- Git owns the index, commits, and remote refs. The skills cannot update plan/backlog completion or bypass the implementation loop.
- The shared workflow reference, Engineering Workflow guide, README, architecture, repository map, and overview illustration describe the new sequence and count.

## Decisions

- Each invocation authorizes only its named operation when scope is clear, subject to platform permissions. Skills report their result and stop without chaining.
- ADD uses explicit relevant paths, includes deletions, preserves unrelated staging and edits, and asks before changing mixed-purpose or uncertain partial staging.
- COMMIT reviews the index, shows an accurate message, creates one ordinary commit with normal hooks/signing, and verifies its ancestry and contents. It never stages or pushes. Explicit `message only` or preview requests retain read-only drafting.
- PUSH requires a clean working folder and attached branch. It resolves the upstream or an unambiguous origin/current-branch destination, obtains fresh evidence from the effective destination, lists all outgoing commits, and verifies the remote tip after an ordinary single-branch push.
- Either a local or destination `main`/`master` branch requires authorization naming that branch. Ambiguous destinations and rejected operations stop without automatic recovery, force-pushing, tag pushing, amending, hook bypass, pulling, or rebasing.

## Test Impact

- All five steps passed VERIFY, the engineer checkpoint, and their scoped TEST validation before their plan checkboxes were updated. The completed plan is [git_finalization_skills_plan.md](../plans/git_finalization_skills_plan.md).
- The skill-creator validator passed for ADD, COMMIT, and PUSH. Documentation checks verified local links/anchors, all 24 folder/frontmatter names, exact catalog membership, installation counts, workflow/map references, and unchanged implementation-gate contracts. Formatting checks passed.
- Disposable local Git repositories exercised explicit-path additions/edits/deletions, unusual filenames, pre-staged and partially staged work, already-staged no-ops, and preservation after staging failure.
- Commit checks covered initial and staged-only commits, preserved unstaged edits, empty staging, read-only inspections, rejecting hooks, and detection of unexpected successful-hook changes. A staging-test expectation was corrected through DEBUG before the test passed; no skill change was needed.
- Push checks used a local bare remote: first push/upstream setup, differently named upstreams, multiple outgoing commits, synchronized comparisons, new refs with already-present commits, stale refs, remote-ahead/diverged history, rejected pushes, and disabled automatic tag following. Dirty/detached and conflicting-endpoint facts were also inspected.
- Ambiguity, unrelated-scope rejection, message-only selection, and main/master authorization policies were reviewed through instruction walkthroughs. The temporary Git checks validate command mechanics, not autonomous agent compliance with every instruction.
- The overview PNG passed structural/CRC checks, retained its 1254 x 1254 dimensions, and was visually compared with the original for count, text, icons, layout, and quality.
- Final read-only security and performance reviews found no actionable issues. No test push was made to GitHub.

## Deployment Notes

- Copy the two new skill folders and updated commit contract alongside the updated shared workflow document when updating an adopting repository. Start a fresh Codex chat to discover copied skills.
- The commit default intentionally changed: use `$turtle-commit message only` for the former read-only behavior.

## Follow-ups / Accepted Risks

- These are procedural contracts; execution depends on the agent, Git configuration, repository hooks, remote protections, and platform permissions.
- Validation helpers remained temporary; no permanent test framework or continuous-integration workflow was added.
