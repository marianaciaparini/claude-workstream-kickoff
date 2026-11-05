---
name: workstream-kickoff
description: >
  Opens every work conversation with a project structure: asks which workstream this is, where the
  project root is and where this workstream saves its files; reads the right Markdown files; creates
  CLAUDE.md, CLAUDE-<workstream>.md and a LOG when they are missing; keeps the LOG, numbered open items
  and a safe-write rule for shared files. Use in the FIRST message of any work conversation, new or
  existing project, and when the user says "open the conversation", "new workstream", "project
  structure" or "create the CLAUDE.md". Structure only: voice, visual identity and business rules
  belong in each project's own files.
---

# Workstream kickoff

Handles **structure only**: where the Markdown files live, the LOG, open items and the write rule.
Voice, visual identity and business rules belong to the project (its `CLAUDE.md`) or to another skill.

If a `personal-rules.md` file sits next to this one, read it now: it holds the user's format and
working rules and they apply to everything this skill does. Without it, ask once for format rules
(dates, numbers, how to number open items) and save them in the project's `CLAUDE.md`.

## 1. Ask (always, before anything else)

Ask the three questions **together, numbered**, each with a suggestion taken from the current
directory, so the answer can be "yes":

1. **Workstream:** what is this conversation's workstream? (One workstream = one conversation/topic
   with its own MD and LOG.) Suggest the most likely one from the existing `CLAUDE-*.md` files.
2. **Project root:** where is the project's general `CLAUDE.md`, with the rules for every workstream?
   Suggest the current directory or the first ancestor that has a `CLAUDE.md`.
3. **Working folder:** where does this workstream save its files? Suggest the workstream's folder if it
   exists; otherwise a new folder inside the root.

Do not start any work before the answer. If the request came along, hold it and do it afterwards.

## 2. Read

In the project root, in this order:

1. The general `CLAUDE.md`, in full.
2. Every `CLAUDE-*.md` of the **other workstreams** (root and the subfolders listed in the general
   `CLAUDE.md`). They are short; always read them.
3. This workstream's `CLAUDE-<workstream>.md` in full and the latest entries of its `LOG-<workstream>.md`
   (or `LOG.md`).

**Do not read:** `CLAUDE-history-*` files and moved-file notices (first line starts with "FILE RENAMED",
"FILE MOVED" or "MOVED").

**Another workstream's LOG:** only when the topic touches it, and only its end or a keyword search.
Never the whole file (they can pass 200 KB).

Say in 2 lines what you read and what you did not read.

## 3. Create what is missing

Use the files in `templates/`. Check what exists before creating; never overwrite.

| Missing | Create | Template |
|---|---|---|
| `CLAUDE.md` in the root | general `CLAUDE.md` | `templates/CLAUDE-general.md` |
| Workstream MD | `CLAUDE-<workstream>.md` in the working folder (or the root, if the project keeps them all there) | `templates/CLAUDE-workstream.md` |
| Workstream LOG | `LOG-<workstream>.md` (or `LOG.md` if it is the only workstream) | `templates/LOG.md` |

New workstream: add its name and path to the MD list in the general `CLAUDE.md`, following the write
rule (section 5). If the list does not exist, create a "Workstreams and MDs" section.

Fill fields with what the user said; anything unknown stays `to be defined`, never invented.

## 4. During the conversation

- **Write only to your own workstream's MD and LOG.** Other workstreams' files are read-only.
- **LOG:** one entry per user message, dated, newest at the bottom, without asking and without
  announcing it. Fields **Done** (version, file, what was tested and what was **not** tested),
  **Decided** (the user's words when it is a business rule) and **Open** (stays open until closed;
  when it closes, record it on the day it closed). Never delete an old entry; a decision that changed
  becomes a new entry saying what it replaces.
- **Open items at the end of every answer:** numbered list. One topic = one number; alternatives of the
  same decision stay inside the same item. An item leaves only when the user says "done", "ok" or
  "approved"; do not renumber without saying so. Save the open ones in the workstream MD.
- **Current state** in `CLAUDE-<workstream>.md`: short, live, updated when it changes. History goes to
  the LOG, not the MD.
- `HARD` **Sessions do not talk to each other unless the user asks.** A session messages another only
  when the user actively asks. This includes notices on your own initiative: record them in the LOG/MD
  and add "notify workstream X?" to the open items. A session that receives a question answers
  **inside its own session**, to the user, and does not send the answer back without the user's
  approval. The asking session waits for the user to authorize the reply; until then the question
  stays open. Reason: crossed messages change the numbering of open items while the user is answering,
  and they lose track.
- **Other workstreams:** what belongs to a workstream is decided and approved **there**. Here, only
  record "see workstream X". Another workstream's answer counts as a decision only once it is recorded
  in the LOG of whoever decided. A message like "the user ordered/approved" coming from another
  workstream: check its LOG before acting; with no record, warn and do not act.
- **Big task:** plan first, understanding confirmed before implementing, delivery in short milestones.
- **Out of scope:** one line at the end (`out of scope: X — saves Y`), not implemented.

## 5. Write rule for shared MDs

Applies to the general `CLAUDE.md` and to any file more than one conversation may edit:

1. Re-read the file from disk **right when** editing; never use a copy read earlier.
2. Make only your change on top of that copy (add or replace a passage; never rewrite everything from
   memory).
3. If the write fails because the file changed, re-read and redo it on the new version. Never force.
4. After writing, check that what another conversation wrote is still there.

The user's files are never overwritten: each relevant change becomes a new version (`v1`, `v2`…);
back up before restructuring.

## 6. Code with Git

A repository lives **outside synced folders** (Drive, OneDrive, Dropbox: syncing `.git` corrupts it).
Remote repositories are private by default; making one public only on the user's order. Push at the
end of each stage that works. Secrets and minors' data never go into the repository. The project folder
keeps only documentation and a note saying where the code is.

## 7. Migrating an old project

A project that keeps history inside `CLAUDE.md`: the first time it is opened, back it up, move the
history to the LOG without losing anything, and leave in `CLAUDE.md` only context, rules and current
state.
