# Fleet Radar

Always-on big-screen view of your whole agent fleet.

A [workspacer](https://github.com/DJTouchette/workspacer) hub plugin (webview). Point it at a spare monitor and glance over to see, at any moment, who's working, who's blocked on you, and what the whole fleet is costing.

## What it does

A polished, TV-style card grid of every live agent. On any subscribed bus event it debounces (~150ms) and re-polls `agents.list`, then re-renders a responsive grid where each card shows:

- the agent **name** (its `cwd` basename, or a short `sessionId`),
- a **colored state dot** — green idle, blue thinking/responding, amber waiting,
- the **model**,
- a **context-usage bar** (blue → amber → red as it fills),
- the running **cost**.

Agents that need you — `waiting_approval` / `waiting_input`, or with a pending approval/question — are pulled to the top and lit with a glowing "needs you" / "approve" ring, and the header shows a live count of how many are blocked. The header also totals the agent count and combined cost. **Click any card** to focus that agent in workspacer (`command.focus_agent`). A clean radar empty state shows when nothing is running.

## Bus wiring

- **Subscribes to:** `agent.snapshot`, `agent.state_changed`, `workflow.*`
- **Calls capabilities:** `agents.list`
- **Emits:** `command.focus_agent`

## Run it

1. Copy this folder to `~/.config/workspacer/plugins/fleet-radar/` (or install from GitHub via the workspacer command palette → *Install from GitHub…* → `DJTouchette/workspacer-plugin-fleet-radar`).
2. Reload plugins in workspacer.
3. Open the **Fleet Radar** pane (`ctrl+shift+r`).

## Implement

All logic lives in `ui/index.html` (a bus-native webview). It subscribes to `agent.snapshot`, `agent.state_changed`, and `workflow.*`; each event triggers a single debounced `call('agents.list')` refresh so event bursts collapse into one re-render. The roster is read straight from the `agents.list` rows (`sessionId`, `cwd`, `state`, `model`, `contextTokens`, `contextLimit`, `costUSD`, `pendingApproval`, `pendingQuestions`). Clicking a card `publish`es `command.focus_agent` with that agent's `sessionId`.

The webview only runs while its pane is open. Colors come from the injected `--wks-*` theme variables (with standalone fallbacks); no plugin settings are used.

## Layout

```
fleet-radar/
  plugin.json      # manifest (events + capabilities)
  ui/index.html    # bus-native webview; implement onEvent()
  README.md
```

## License

MIT
