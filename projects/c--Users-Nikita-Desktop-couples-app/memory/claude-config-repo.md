---
name: claude-config-repo
description: "The user's ~/.claude is itself a private git repo, synced between their Windows machine and their Mac"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 0630036d-8801-4919-93f1-43949202295b
  modified: 2026-08-28T14:36:11.836Z
---

`~/.claude` is a git repository: `https://github.com/Pais02/claude-config` (private, GitHub account `Pais02`, branch `master`). Set up 2026-08-28 so the Windows machine and the user's Mac share one configuration.

Tracked: `CLAUDE.md`, `settings.json`, `rules/`, `skills/`, `agents/`, and `projects/*/memory/`. `.gitignore` is a **whitelist** — everything is ignored unless named — so credentials, caches, session transcripts and plugin checkouts stay out even when Claude Code invents new filenames.

**How to apply:** after editing a rule, skill, subagent or memory file, those edits are already in a working tree — commit and push them so the other machine gets them, and `git pull` there before relying on them. Never loosen the whitelist into a blacklist; `.credentials.json` sits in the same directory.

**Memory does not relocate on its own:** `projects/` subdirectories are named after the project's absolute path, so `c--Users-Nikita-Desktop-couples-app` is meaningless on macOS. Open the project once on the new machine to let Claude Code create the right directory, then copy `memory/` into it.

See [[project-overview]] for the Alcove project itself, whose code lives in a separate private repo.
