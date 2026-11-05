# Workstream kickoff — a Claude skill for running big projects as parallel AI sessions

I run several long projects with Claude at the same time: a bookkeeping product, a school's admin
system, a food-service company's operations. Each project has many fronts: code, brand, legal, data,
dashboard, website. Keeping all of that in one chat doesn't work. The context gets too long, decisions
get lost, and two sessions end up editing the same file.

So I split each project into **workstreams**, with one Claude session per workstream, and wrote this
skill to make every session start the same way.

## What it does

At the first message of any work conversation, the skill:

1. **Asks three numbered questions** (workstream, project root, working folder), each with a
   suggested answer taken from the current directory. Most of the time the answer is just "yes".
2. **Reads the right files:** the project's general `CLAUDE.md`, the short MD of every other
   workstream, and this workstream's MD plus the latest LOG entries. It skips history files and
   never loads a whole log from another workstream.
3. **Creates what is missing** from templates, without overwriting anything.
4. **Keeps a LOG** with one entry per user message: what was done, what was tested and what was
   **not** tested, what was decided (in the user's own words), and what is still open.
5. **Ends every answer with numbered open items**, one topic per number, so the user can reply
   "1 yes, 3 no" and nothing gets lost.

## The rules that make parallel sessions safe

- **Each session writes only to its own MD and LOG**; it reads everyone else's.
- **Sessions don't message each other unless the user asks.** Crossed messages renumber open items
  while the user is answering. Approvals are checked against the LOG of whoever approved them.
- **Shared files use a safe-write rule:** re-read from disk right before editing, change only your
  part, never force a write over someone else's change, and verify afterwards.
- **Nothing is invented:** unknown fields stay `to be defined`.
- **Git repos live outside synced folders** (Drive or OneDrive sync corrupts `.git`), and secrets and
  minors' data never go into a repository.

## Structure

```
workstream-kickoff/          English version of the skill
  SKILL.md
  templates/
    CLAUDE-general.md        project-wide rules and the list of workstreams
    CLAUDE-workstream.md     one per workstream: scope, current state, open items
    LOG.md                   append-only log, newest at the bottom
pt-BR/abertura-de-conversa/  original Portuguese version (the one I use daily)
```

## Install

Copy the `workstream-kickoff` folder to `~/.claude/skills/` (Claude Code) and start a new conversation.
To keep your own format rules (date format, number format, tone), add a `personal-rules.md` file next
to `SKILL.md`. The skill reads it first.

## Results so far

I use it daily on two large projects with about 10 workstreams each. A new session opens with the
rules, the current state and every open item from the last one, so nothing depends on my memory.
