# omarchy-harpoon

Hotkeys for [Omarchy](https://omarchy.org)'s Hyprland, like Raycast's: one key per app or per
scratchpad workspace, plus a card naming the workspace you land on.

- **jump**: go to an app. On this workspace first, else the next workspace to the right that
  has it; pressed again on the app, its next window, left to right, wrapping. A key only ever
  lands on its own app, and never moves a window.
- **overlay**: a scratchpad workspace of its own (`special:harpoon-<name>`), shown over the
  current one and hidden by the same key. Its main app's windows are pulled in from wherever
  they are, any other window opened while it's shown stays in it, and the key brings it back
  on the window last used there, whatever app that is. Empty, it launches its main app.
- **the workspace card** (`Toast.qml`, an Omarchy shell plugin): names the workspace you land
  on, low on the screen, after any workspace change.

## Install

```sh
omarchy plugin add https://github.com/josemiguelo/omarchy-harpoon.git --enable
ln -sf ~/.config/omarchy/plugins/josemiguelo.harpoon/hypr/harpoon.lua ~/.config/hypr/harpoon.lua
```

The plugin brings the card; the link lets Hyprland load the engine as `hypr.harpoon`, so
`omarchy plugin update josemiguelo.harpoon` updates both (reload Hyprland for the engine).

## Keys

In `~/.config/hypr/bindings.lua`:

```lua
local harpoon = require("hypr.harpoon")
local hyper = "SUPER + CTRL + ALT + SHIFT + "
harpoon.bind(hyper .. "T", { name = "Terminal", class = "^kitty$", launch = "omarchy-launch-terminal", mode = "jump" })
harpoon.bind(hyper .. "B", { name = "Browser", class = "^brave%-browser$", launch = "omarchy-launch-browser", mode = "jump" })
harpoon.bind(hyper .. "A", { name = "AI", class = "^brave%-claude%.ai__%-Default$", launch = "omarchy-launch-webapp https://claude.ai/", mode = "overlay" })
harpoon.bind(hyper .. "M", { name = "Media", class = "^[Ss]potify$", launch = "omarchy-launch-spotify", mode = "overlay" })
```

`class` is a Lua pattern on the window class, or a list of them. An overlay's `name` names
its workspace; its `class` and `launch` are its main app.

Show the card by hand: `omarchy-shell -q harpoonToast show '{"workspace":"2"}'`.
