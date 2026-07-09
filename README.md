# Fleet Radar

Always-on big-screen view of your whole agent fleet.

A [workspacer](https://github.com/DJTouchette/workspacer) hub plugin (webview). **Runnable scaffold** — it loads, connects to the hub bus, and shows live activity; the real logic is stubbed with clear TODOs.

## What it does

A TV-style grid of every agent — who's working, who needs you, live cost/context — driven by `agent.snapshot` + `agent.state_changed`. Click an agent to focus it in workspacer (`command.focus_agent`).

## Bus wiring

- **Subscribes to:** `agent.snapshot`, `agent.state_changed`, `workflow.*`
- **Calls capabilities:** `agents.list`
- **Emits:** `command.focus_agent`

## Run it

1. Copy this folder to `~/.config/workspacer/plugins/fleet-radar/` (or install from GitHub via the workspacer command palette → *Install from GitHub…* → `DJTouchette/workspacer-plugin-fleet-radar`).
2. Reload plugins in workspacer.
3. Open the **Fleet Radar** pane (`ctrl+shift+r`).

## Implement

Edit `ui/index.html` → `onEvent(event)`. The webview only runs while its pane is open; use `call('method', params)` for capabilities and `publish('command.x', data)` for commands.

## Layout

```
fleet-radar/
  plugin.json      # manifest (events + capabilities)
  ui/index.html    # bus-native webview; implement onEvent()
  README.md
```

## License

MIT
