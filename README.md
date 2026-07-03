# Agent Memories

A portable, project-agnostic memory and skill scaffold for AI coding agents (Claude Code, Gemini/Antigravity, and similar).

The core idea: an agent that works across many different codebases accumulates two kinds of knowledge —
knowledge that is useful *everywhere* (how you like to communicate, how you like commits done, reusable
tool workflows) and knowledge that only makes sense for *one specific project* (its language, its build
system, its domain quirks). Mixing the two makes the "everywhere" knowledge unusable when you switch to a
different project. This repository keeps only the first kind. Project-specific knowledge lives inside each
project's own repository instead.

## Structure

```
Agent_Memories/
├── Claude_Memory/          # Claude-specific working memory
│   ├── MEMORY.md            # index — start here
│   ├── claude_info/         # CHECKPOINT / NOTES / TIPS / TODO (session-scoped scratch files)
│   ├── claude_log/          # dated work logs (LOG_YYYYMMDD_<topic>.md), format only — see README inside
│   ├── claude_skills/       # mirrored Skill definitions (archive-session, graphify, grill-me, handoff)
│   ├── feedback_*.md        # behavioral rules learned from user corrections/confirmations
│   ├── user_*.md            # stable facts about the user (preferences, profile)
│   └── reference_*.md       # reusable tool/reference documentation (e.g. graphify usage)
├── caveman/                 # ultra-compressed communication mode (skill + subcommands)
├── html-slide-deck/         # skill for building self-contained HTML slide decks
└── html-slide-deck.skill    # packaged version of the above
```

### Memory file conventions (`Claude_Memory/`)

| Prefix | Meaning | Changes how often? |
|---|---|---|
| `feedback_*.md` | A behavioral rule learned from an explicit user correction or confirmation. Each file states the rule, **why** it exists, and **how** to apply it. | Rare — only when the user corrects or confirms an approach |
| `user_*.md` | Stable facts about the user: preferences, working style, profile. | Rare |
| `reference_*.md` | Documentation for a reusable tool or workflow that isn't tied to any one project. | Occasional |
| `claude_info/*.md` | Short-lived session scratch space (current status, discoveries, todos). | Every session |
| `claude_log/*.md` | Dated logs of non-obvious discoveries, kept for future reference. | Per notable discovery |

**Rule of thumb**: if a note only makes sense to someone who already knows the specific project you were
working in, it does not belong here — move it into that project's own memory/notes instead. This repository
was extracted from a working setup that originally mixed both kinds of content; project-specific material
was moved out during that cleanup.

## Usage

1. Clone or copy this repository's contents into wherever your agent looks for persistent memory
   (for Claude Code / Cowork-style setups, that's typically a fixed path referenced from your global
   `CLAUDE.md`).
2. Point your agent's session-start instructions at `Claude_Memory/MEMORY.md` — it's the index and links to
   everything else.
3. As you work, let the agent keep `claude_info/` and `claude_log/` current. When a note turns out to be
   project-specific, move it into that project's own memory instead of leaving it here.
4. Skills under `caveman/` and `html-slide-deck/` can be installed/loaded independently of the memory
   convention above; they're self-contained.

## Notes

- `graphify-out/` (a generated knowledge-graph cache used by the `graphify` skill) is intentionally
  git-ignored — it's regenerated per-project on demand and shouldn't be committed.
- This repo assumes no particular OS, language, or toolchain. If you find something in here that only makes
  sense for one specific stack, that's a bug — please move it out.

## Commit Convention

Commit messages in this repository follow the [Angular commit message
convention](https://github.com/angular/angular/blob/main/contributing-docs/commit-message-guidelines.md):

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

- **type**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`
- **scope**: optional, whatever area of the repo the change touches (e.g. `feedback`, `caveman`, `readme`)
- **subject**: imperative, present tense, no capital first letter, no trailing period
- Lines wrapped at 100 characters
- Reverts start with `revert: ` followed by the original commit's header, with `This reverts commit <hash>.`
  in the body
- Breaking changes are called out with a `BREAKING CHANGE:` line in the footer

## License

MIT — see [LICENSE](LICENSE).
