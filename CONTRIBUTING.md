# Contributing

Thanks for improving the herdr skill. This file is short on purpose —
the goal is to keep the change/PR loop small.

## What lives here

Exactly one skill, in [`skill/`](./skill):

- `skill/SKILL.md` — the skill itself, loaded by the agent.
- `skill/reference.md` — verbatim CLI/socket reference (rarely edited).

Plus the surrounding repo glue: `README.md`, `CHANGELOG.md`, `scripts/`.

## Adding a new skill

This repo currently hosts only the herdr skill. If you want to add a
second skill:

1. Create `skill-<name>/SKILL.md` (and optional `skill-<name>/reference.md`)
   next to the existing `skill/`.
2. Add a top-level entry in `README.md` with a one-line description and
   a trigger list.
3. Add a `CHANGELOG.md` entry under a new minor version.
4. Bump the version in the YAML frontmatter of every affected `SKILL.md`.

If skills start to multiply, split this repo into
`hermes-skill-<name>` repos under the same `machine-machine` org. Keep
this one focused on herdr.

## Editing the existing skill

1. **Edit `skill/SKILL.md`** — the source of truth. Keep prose tight;
   agents load this on every invocation.
2. **Cross-references** — if you add a new section, link it from the
   Quick Reference table at the bottom of `SKILL.md`.
3. **Frontmatter** — the `description` field is what triggers skill
   load. Treat it as part of the contract: it must list the words
   and phrases that should make a host agent load this skill.
4. **Bump the version** in `skill/SKILL.md` frontmatter (`version:`
   field) and add a CHANGELOG entry:
   - **PATCH** for typos, clarifications, broken links.
   - **MINOR** for new workflows, new sections, new examples.
   - **MAJOR** for breaking changes to the workflow or command
     examples agents follow.
5. **Run the linter**:
   ```bash
   scripts/lint.sh
   ```
   It checks frontmatter shape, broken relative links, and that
   the version field matches the latest CHANGELOG entry.
6. **Update your local install** if you use the skill from
   `~/.hermes/skills/` or `~/.claude/skills/`:
   ```bash
   scripts/install.sh --local
   ```

## Commit messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` — new workflow, new section
- `fix:` — correction to existing workflow
- `docs:` — README, CHANGELOG, CONTRIBUTING only
- `chore:` — scripts, license, .gitignore
- `refactor:` — re-organizing without semantic change

The body should reference the section number when relevant
(e.g. `Refs §9.2 — clarify --no-focus behavior`).

## PR review

- One approver from the `machine-machine` org.
- CI is not currently wired up; `scripts/lint.sh` is the only gate.
- Squash-merge to keep `main` linear.

## Style notes for `SKILL.md`

- Imperative voice ("Spawn the herd", not "Spawning the herd").
- Code blocks must be runnable as-is — they go straight into agent
  bash, so test them.
- Always cite the herdr CLI surface in `reference.md` rather than
  duplicating long flag tables in `SKILL.md`.
- Tables beat prose for "what command does what".

Thanks again.
