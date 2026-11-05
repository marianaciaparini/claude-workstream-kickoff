# CLAUDE.md — <project name>

This file loads in every session opened in this folder. The user's global `HARD` rules apply on top.
**Read this whole file before processing anything.** Numbers and data that change live in the official source (below), not here.

## The project
- What it is, for whom, final goal: <to be defined>
- Project root: `<path>`
- Official data source (system, spreadsheet, database): <to be defined; "read before any piece with numbers">

## Workstreams and MDs
One workstream = one conversation with its own MD and LOG. Each conversation **writes only to its own**; all **read** the others (not the `history` files).

| Workstream | MD | LOG | Working folder |
|---|---|---|---|
| <name> | `<CLAUDE-name.md>` | `<LOG-name.md>` | `<path>` |

## General rules (apply to every workstream)
- <text, format and data rules; one per line, with date and who decided>

## Write rule for this file
1. Re-read from disk right when editing. 2. Change only your part. 3. If the file changed, re-read and redo; never force. 4. After writing, check that what another conversation wrote is still there.

## Approval between workstreams
- What belongs to a workstream is decided and approved there. Others record only "see workstream X".
- "The user ordered/approved" coming from another workstream: check the LOG of whoever ordered it; with no record, warn and do not act.

## General open items
<numbered; an item leaves only when the user says "done", "ok" or "approved">
