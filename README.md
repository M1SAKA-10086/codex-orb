# Codex Orb

A standalone Windows floating orb for Codex Desktop. Task time in the center; two labeled rings show remaining 5-hour and weekly account quota.

[中文说明](README.zh-CN.md) · [Download](https://github.com/M1SAKA-10086/codex-orb/releases/latest)

![Orb preview with demonstration values](preview.png)

- 300 × 300 orb below the conversation title; drag to move and remember placement.
- Automatically hides when Codex leaves the foreground, its main window closes/minimizes, or the task finishes. Returns when Codex is foreground and a task remains active; elapsed time continues in the background.
- Teal outer ring: **5h** remaining quota. Purple inner ring: **weekly** remaining quota.
- Reads the current model and reasoning effort from the visible model picker, independently of the model that executed the current turn.
- Timer and quota can be toggled separately from the tray. Optional Windows logon startup.
- Works independently of Codex Slash, ASUS hardware and G-Helper. No lighting or HID code is included.

## Run

Download `CodexOrb-1.0.0-win-x64.zip`, extract it and launch `CodexOrb.exe`. The small portable build requires **.NET 10 Windows Desktop Runtime (x64)**. No administrator privileges are required. Keep `settings.json` beside the executable.

The app stays in the notification area. Right-click its tray icon for timer/quota switches, startup, status and exit. Right-click the orb to hide it; re-enable its contents from the tray. Double-click the orb to open status. Enable logon startup only after moving the folder to a permanent location.

If Codex Slash is also running, turn off its two overlay switches to avoid two overlapping orbs. Codex Orb does not change Codex Slash settings.

## Data and limits

Only local Codex JSONL sessions and the Windows accessibility model-picker/header controls are read. No credentials, hooks, API requests or server connection are required. The orb makes no network calls. Its settings, saved placement and diagnostic status remain under `%LOCALAPPDATA%\CodexOrb`.

Quota percentages are **account-wide remaining limits**, not this task's exact cost. Values refresh when Codex writes a quota event; expired or missing data stays unknown. Local JSONL formats and accessibility controls are implementation details and may require updates after Codex changes.

Task time starts at the real `task_started` event. When multiple chats are active, the most recently active primary task is used; `ThreadId` can pin a thread. Subagents are excluded. The adapter scans the newest 24 session files and treats tasks without events for 10 minutes as idle. The header follows the visible conversation; pin the intended thread when switching among simultaneous tasks. Other Codex UI languages/layouts and multiple main windows have not been validated.

`CODEX_HOME` is supported. You can set `CodexHome` explicitly in `settings.json`; omit it to use the default. Manual timer/quota switches persist during the current Windows login; a new login enables both by default. Dragged position persists across logins.

## Build and verify

Requires the .NET 10 SDK on Windows.

```powershell
./build.ps1 -Output ./artifacts
./artifacts/CodexOrb.exe --self-test ./self-test.json
```

`-SelfContained` builds a larger package with the runtime included. `--preview path.png` renders an orb with demonstration values. `--smoke-test result.json` runs the real tray application for eight seconds, saves local state and exits. Tests require no Codex login or hardware; a live smoke test only reads local state.

See [TEST-RESULTS.md](TEST-RESULTS.md) for the validation performed for this release. Distributed under GPL-3.0; see [LICENSE](LICENSE) and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md). This is an independent companion utility, not an official OpenAI product.
