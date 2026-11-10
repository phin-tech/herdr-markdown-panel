---
name: herdr-markdown-panel
description: Install, update or dock the Herdr Markdown panel mod (github.com/phin-tech/herdr-markdown-panel), a Markdown viewer and editor with fuzzy file search that opens as a floating window, a split pane or a layout sidebar. Use when asked to install or update the Markdown panel in Herdr, put it in the sidebar or a layout, set up md-tab or md-review, or when the panel doesn't show or doesn't pick up a change.
---

# Herdr Markdown panel

A Herdr mod in `mods/markdown-panel` of github.com/phin-tech/herdr-markdown-panel. It opens three ways:

| Where | Opened by | Close does |
|---|---|---|
| Floating window | `cmd-shift-m` (`mod.open:markdown-panel`) | closes the window |
| Split pane | `cmd-alt-m` (`pane.split` with `mod`) | closes that pane |
| Sidebar | a block in the layout with `props: {docked: true}` | hides the layout's `panel` column |

`$HOME_DIR` below is `${HERDR_APP_HOME:-$HOME/.config/herdr-app}`. Read `herdr-app --skill` before driving the
window; the person's app and core are theirs, so leave restarting them to the person.

## Install or update

1. **Source.** Use the person's checkout if they have one; otherwise clone to
   `~/.local/share/herdr-markdown-panel`, where it stays, since `md-tab` is linked from it. To update, `git pull` there.
2. **Mod.** Copy `mods/markdown-panel` to `$HOME_DIR/mods/markdown-panel`, replacing the folder if it is there. Copy
   it: Herdr hot-reloads a mod folder on save but does not follow a symlinked one, so a linked mod stays on the
   build it started with until the app restarts.
3. **Keys.** Merge the `global` entries of the repo's `keys.json` into `$HOME_DIR/keys.json`. Where the person already
   binds `cmd-shift-m` or `cmd-alt-m` to something else, ask which they keep.
4. **md-tab.** Link `bin/md-tab` into a folder on `PATH` (`~/.local/bin`). It needs `jq` and `bash`.
5. **Check.** Done when `herdr-app check $HOME_DIR/mods/markdown-panel` passes and `herdr-app status` shows
   `"errors": []`. A broken save keeps the old build running and lists the error there.

## Dock it in the sidebar

The shown layout is `herdr-app status | jq -r .layout`, its file `$HOME_DIR/layouts/<name>.json`.

1. **A layout of the person's own.** Herdr's shipped layouts (listed as `layout:<name>` in `$HOME_DIR/installed.json`)
   are replaced on every start. For one of those, clone it first:
   `herdr-app do layout.clone '{"name":"main","as":"<name>"}'`, which also switches to the clone.
2. **The block.** In layouts cloned from `main`, the right-hand panel is a column `panel` holding a stack
   `panel-body` (`files`, `changes`). Add this child to `panel-body`:
   ```json
   { "id": "markdown", "type": "block", "content": { "mod": "markdown-panel", "props": { "docked": true } } }
   ```
   The layout reloads within a second of the save.
3. **Opening it.** `herdr-app do layout.select --stack panel-body --child markdown && herdr-app do layout.show --id panel`.
   Herdr's shipped tab bar (`tabs`) has buttons for Files and Changes only. For a Markdown button, clone it
   (`herdr-app do mod.clone '{"name":"tabs","as":"<name>"}'`), point the layout's tab-bar block at the clone, and add
   a button that runs those two commands, shown when `panel-body` has a `markdown` child.
4. **Check.** Done when the panel shows in the sidebar listing the focused pane's folder, and switching to a tab in
   another folder changes the list within a second.

Docked, the panel follows the focused pane's folder, leaves keyboard focus where it is, and leaves files opened
from outside (`md-review`, the Finder handler) to the floating window.

## Opening files

- **From a script:** `herdr-app do pane.split --direction right --mod markdown-panel --props '{"file":"/abs/path.md"}'`;
  `tab.new` takes the same `mod` and `props`.
- **A whole tab:** `md-tab --name <tab> --file /abs/doc.md --agent claude --task "..."` builds agent, panel and shell.
- **A review an agent waits on:** `mods/markdown-panel/md-review /abs/doc.md` opens it, waits until the person
  presses Done, and prints their edits as a diff. Run it inside a Herdr terminal; `md-review --rule` prints the rule
  to give agents.
- **From outside:** write the absolute path to `$HOME_DIR/data/kanso-open/<name>.path`, then open the panel.
