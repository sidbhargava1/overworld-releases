# Overworld releases

Signed builds of Overworld, a Mac app that shows Claude Code's agents as residents of a small town. It only
observes: it never blocks, approves or changes anything in Claude Code. This repo holds the DMGs (under Releases)
and `appcast.xml`, the feed installed copies check once a day.

## Install (macOS 15 or later, Apple silicon)

1. From Releases, download `Overworld-<version>.dmg`.
2. Open it, drag Overworld onto Applications, eject.
3. Open it. It is self-signed, not notarized, so macOS blocks the first launch: dismiss the warning, then
   System Settings → Privacy & Security → **Open Anyway**.

Without admin rights, install into your own Applications folder instead:

```sh
xattr -d com.apple.quarantine ~/Downloads/Overworld-<version>.dmg
hdiutil attach ~/Downloads/Overworld-<version>.dmg
mkdir -p ~/Applications && cp -R /Volumes/Overworld/Overworld.app ~/Applications/
hdiutil detach /Volumes/Overworld
xattr -dr com.apple.quarantine ~/Applications/Overworld.app
open ~/Applications/Overworld.app
```

## Updates

Overworld checks `appcast.xml` daily. It shows an alert only while you are already looking at the app; otherwise the
app menu and the menu bar item read "Update to <version>…" and the alert opens when you click one. Each DMG carries an
EdDSA signature that the installed app verifies before installing. The check is a plain request for `appcast.xml`: no
query parameters, no system profile. Offline, it fails quietly.
