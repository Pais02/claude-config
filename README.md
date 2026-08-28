# Claude Code configuration

The parts of `~/.claude` worth keeping: global instructions, rules, skills,
subagents, settings, and per-project memory. This repository *is* `~/.claude`,
so edits made while working are already in the working tree.

Everything else in that directory — credentials, caches, session transcripts,
plugins, machine state — is ignored by a whitelist in `.gitignore`, so a new
file Claude Code drops in stays out unless it is named there deliberately.

## What is here

```
CLAUDE.md              loads rules/voice-and-terminology.md
settings.json          model, theme, effort level
rules/                 how to behave: plan-do-verify, tone, voice-input quirks
skills/                commit, deploy, docker, install-mcp, proxy, ssh-connect,
                       tavily, worktree
agents/                subagents: research, reviewer, qa, docs, ...
projects/*/memory/     what Claude remembers per project
```

## Setting this up on another machine

Install Claude Code and sign in first — `claude login`. That creates
`~/.claude` with a credential file, which must stay out of git.

Then adopt this repository into that directory:

```sh
cd ~/.claude
git init
git remote add origin https://github.com/Pais02/claude-config.git
git fetch origin
git reset --hard origin/master
```

`reset --hard` overwrites tracked files only — the freshly generated
`settings.json` is replaced by this one, which is the point. Credentials,
caches and anything else untracked are left alone.

From then on it is an ordinary repository: `git pull` to take changes from the
other machine, commit and push to send them back.

## Memory does not move by itself

`projects/` subdirectories are named after the absolute path of the project
they belong to, so `c--Users-Nikita-Desktop-couples-app` means nothing on a
Mac and its memory will not be found.

Open the project once on the new machine — Claude Code creates the correctly
named directory — then copy the `memory/` folder across into it:

```sh
cp -R ~/.claude/projects/c--Users-Nikita-Desktop-couples-app/memory \
      ~/.claude/projects/<the-new-name>/
```

## Known snags

`rules/голосвой ввод.md` is a byte-for-byte duplicate of
`rules/voice-and-terminology.md`, kept deliberately for now. Its name is
Cyrillic and stored decomposed (NFD), which macOS handles natively but Git Bash
on Windows cannot open — tools there will report it missing.

`agents/docs.md` declares `mcp__context7__*` tools, and no context7 MCP server
is configured on any machine. The agent will fail until one is added; see
`skills/install-mcp`.
