---
name: mac-machine-setup
description: "What is and is not set up on the user's Mac, where the iOS half of Alcove is built"
metadata:
  type: project
---

The Mac checkout of Alcove lives at `/Users/nikita/Documents/alcove`. Xcode 26.6, iOS 26.5 simulators. Verified 2026-08-28:

**Works — do not repeat the earlier misdiagnosis.** The simulator is fully usable without `/var/db/xcode_select_link`. That symlink is absent, `xcode-select -p` resolves Xcode by default, and boot / install / launch / `simctl io screenshot` all succeed. An earlier session claimed the missing symlink blocked the simulator and asked the user to run `sudo xcode-select -s`; that was wrong. Building, booting and screenshotting need nothing from the user.

**Still missing:**

- **No Homebrew.** So any advice shaped `brew install X` is a dead end until brew itself is installed — this tripped up an earlier session that suggested `brew install gh` and `brew install node`.
- ~~No Node/npm.~~ **Fixed 2026-09-03.** Node 24.20.0 via nvm in `~/.nvm` — no sudo, no brew. It is not on the default PATH, so a shell needs `export NVM_DIR="$HOME/.nvm"; . "$NVM_DIR/nvm.sh"` first. `db-tests` now run here: `cd db-tests && node run.mjs`.
- ~~No GitHub credentials.~~ **Fixed 2026-09-03.** An ed25519 key at `~/.ssh/id_ed25519` is on the account, both remotes are SSH, and pushes to `Pais02/alcove` and `Pais02/claude-config` work. `gh` is still absent; plain `git` is enough.

**Taps are impossible, but not for the symlink reason.** `simctl` has no synthetic-tap command at all (only `io` for screenshots and `ui` for appearance), and `osascript` lacks Accessibility permission to click the Simulator window. So: build ✓, launch ✓, screenshot ✓, tap ✗. Any end-to-end flow needing a tap has to be driven by the user or by a UI test target.

**How to apply:** on the Mac, expect to build, launch and screenshot freely; expect not to tap, push, or run the database tests. See [[project-overview]] and [[claude-config-repo]].
