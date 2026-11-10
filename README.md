# herdr-markdown-panel

A markdown viewer and editor for the [Herdr app](https://herdr.dev), forked from the
`markdown-panel` mod in [jlopezlira/herdr-kanso](https://github.com/jlopezlira/herdr-kanso).

## Changes from upstream

- **Split pane.** Opens beside a terminal as well as in the floating window. Close and Escape close the pane when there is no window.
- **Copy button** above each fenced code block. It copies the code without the fences.
- **Paste in terminal button** above each fenced code block. It pastes into the first terminal pane of the same tab and does **not** press Enter.

## Install

Copy `mods/markdown-panel` to `~/.config/herdr-app/mods/` and merge `keys.json` into `~/.config/herdr-app/keys.json`.

| Key | Does |
|---|---|
| `cmd-shift-m` | Floating window (press again to close) |
| `cmd-alt-m` | Split pane to the right of the focused pane |

To open a given file, write its absolute path to `~/.config/herdr-app/data/kanso-open/<name>.path`, then open the panel.

## What it runs

No network calls. It runs `sh`, `find`, `git ls-files`, `pwd` and `mkdir`, and `recent.sh` reads the clipboard, the focused terminal's screen (`hc read`) and `~/.claude/sessions`. It reads and writes any Markdown file you open. `markdown-handler.sh` (macOS file association) only runs if you run it.

## License

Apache-2.0. See `LICENSE` and `NOTICE` for the upstream authors.
