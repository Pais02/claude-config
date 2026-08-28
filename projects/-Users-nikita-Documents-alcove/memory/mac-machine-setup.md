---
name: mac-machine-setup
description: "What is and is not set up on the user's Mac, where the iOS half of Alcove is built"
metadata:
  type: project
---

The Mac checkout of Alcove lives at `/Users/nikita/Documents/alcove`. Xcode 26.6, Swift 6.3, iOS 26.5 simulators. Four things were missing there as of 2026-08-28, each of which blocks something:

- **No Node/npm.** `db-tests/` — both `npm test` and `npm run test:live` — cannot run on the Mac at all. Backend verification happens on the Windows machine.
- **No GitHub credentials.** No `gh`, no `~/.ssh`, and `credential.helper=osxkeychain` holds no github.com entry, so `git push`/`pull`/`clone` against the private `Pais02/*` repos all fail with `could not read Username`. Work commits locally and waits.
- **`/var/db/xcode_select_link` does not exist.** `xcode-select -p` still answers with Xcode because there is only one, but the simulator integration checks for that symlink and refuses to attach. Fix is `sudo xcode-select -s /Applications/Xcode.app/Contents/Developer` — needs the user's password.
- **No Accessibility permission for `osascript`**, so AppleScript cannot click the Simulator window either. With both of those missing, screenshots work (`xcrun simctl io … screenshot`, and only once `Simulator.app` is open — a headless-booted device renders black) but taps do not.

**How to apply:** on the Mac, expect to build and screenshot but not to tap, push, or run the database tests, until the user clears the items above. See [[project-overview]] and [[claude-config-repo]].
