---
name: overdraft
description: >-
  Use when the user wants an existing skill, prompt, or long instruction doc
  made more efficient (fewer tokens) through a series of reviewed drafts,
  without losing any rule or changing its behavior.
---
# Overdraft

Improve an existing skill or instruction file through five numbered drafts. Each draft builds on the last, is saved separately, and is measured against the original. The goal is fewer tokens loaded per run with the same behavior.

## Rules for every draft

- Work in a new copy for each draft (`<name>-v1`, `<name>-v2`, ...). Never edit the original or an earlier draft.
- Never commit, push, or publish. Give the user the commands to do it themselves, with their preferred git identity.
- Keep the frontmatter byte-identical to the original unless the user says otherwise. Published listings may quote the description.
- Reference files use lowercase kebab-case names and paths relative to the skill folder. Check that every pointer resolves.
- Estimate tokens as characters divided by 4 and say it is an estimate.
- Measure cost per path, not just file size: the core that loads every run, plus what each command or mode reads (for example a quick run, a full run, a review run).
- Finalize files before uploading. Upload each file once, as plain `.md`, to a new folder per draft (for example `<name> draft vN`). Don't trash older uploads without the user's direct OK.
- Any behavior the original didn't have needs the user's approval. List it separately in each report.
- Report each draft with a before/after token table, what changed, anything to approve, and the location of the files.

## Before draft 1: audit

1. Measure each section of the original (characters and estimated tokens).
2. Find what loads every run but is only needed sometimes: long catalogs, rare modes, setup steps for unusual cases, per-language tables, examples.
3. Propose a core-plus-reference-files split with estimated savings, and note runtime costs the file can't fix (for example verbose tool output). Wait for the user's go-ahead.

## Draft 1: split word for word

- Keep the always-needed parts in the core: triggers, commands, ground rules, workflow, common modes, report format.
- Move the rest into reference files without rewording it.
- Add a short "Reference files" list in the core that says exactly when to read each file.
- Update internal cross-references ("see X below") so they point at the right file.
- Check that every non-blank original line appears in the new set of files. List each line that was reworded or dropped.
- Check whether moving text broke a dependency. For example, a rule that common runs need but that now lives only in a rarely read file. Copy it into the core and flag it.

## Draft 2: optimize

- Split large reference files further, so a run reads only the part it needs (for example one file per category).
- Merge duplicate statements into one canonical location and point to it.
- Tighten wording without dropping any condition, exception, path, ID scheme, or safety rule.
- Build a rule checklist from the original, one atomic rule per row, and check by script that every rule is present. Require 100%.
- Flag places where the meaning is kept but the wording changed, and any reminders that were weakened.

## Draft 3: compare and merge

- Compare drafts 1 and 2 section by section. Where the meaning is equal, keep the shorter phrasing.
- Restore cheap safety reminders that draft 2 cut, if they protect the common path.
- Remove anything that repeats what always loads (for example examples covered by the commands table, or repeated pointers).
- Re-run the checklist and the per-path token table.

## Draft 4: rubric review

1. Write a rubric that scores 0-10 on each of these:
   - Rule fidelity
   - Token efficiency
   - Agent reliability: will it open the right file at the right time, and can the conditions be read only one way?
   - Internal consistency: the same terms everywhere and no contradictions
   - Spec compliance
   - Readability for a human maintainer
2. Score the previous draft as a strict adversarial reviewer. Read every file. Check edge cases, such as no git, no changes, a dry run combined with other modes, or a missing saved state.
3. Fix every deduction, re-score, and repeat. Don't inflate scores. If a point can only be earned through real-world testing, say so and leave it unearned.
4. Save the rubric and the score sheet with the draft.

## Draft 5: parity with the original

1. Compare the previous draft with the original by behavior, not wording. Sort every difference into one of four groups: (a) added, (b) removed or narrowed, (c) changed, or (d) reworded only.
2. Revert every (a), (b), and (c) item to the original behavior, using the original wording where that's cheap. Keep only the deviations the user approved and the additions that are the point of the work, and flag each one.
3. Make sure that splitting the file never narrows what a run sees. If a summary stands in for full content, say that the full file is authoritative and must be read whenever it might apply.
4. Save a parity table (each original behavior, its new location, and its status: same, approved deviation, or deliberate addition). Re-run the checklist.
5. Report the token cost of parity honestly, including any path that now costs more than the original.

## After the drafts

- Recommend which draft to ship and explain the trade-off (for example the most savings versus the closest match to the original).
- Record deferred efficiency ideas so they can become a later improvement.
- Remind the user to test the chosen draft in its real environment before publishing.
