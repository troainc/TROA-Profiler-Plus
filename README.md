# TROA Profiler+

TROA Profiler+ is a headless performance-intelligence plugin for Torch Space Engineers servers. It helps operators answer the practical question: **what changed when the server started lagging?**

It requires no client mod, WPF panel, web UI, or legacy Profiler installation. Administration is handled through in-game/Torch commands, one documented XML config, portable CSV/JSON files, and optional Discord incident embeds.

## Release

- Version: `v1.0.0-alpha.1`
- Download: `TROA-ProfilerPlus-v1.0.0-alpha.1.zip`
- SHA-256: `F725D830EE5C9E399322D05A25A65B10D31A9AB6F654D08F12FED42C16FDAF4F`
- Runtime: Torch / .NET Framework 4.8
- Hosts: Windows and Linux-hosted AMP/Wine Space Engineers servers
- Source: private to TROA

## Why It Is Different

- **Production-first:** safe interval sampling instead of permanent deep patches across the entire game loop.
- **Useful summaries:** health scores, rolling reports, incidents, and ranked grid pressure—not only raw timings.
- **Portable evidence:** CSV history and JSON snapshots survive Discord scrollback and help compare server changes.
- **No UI dependency:** fewer update failures and consistent operation on remote AMP servers.
- **Privacy-aware:** Discord incidents report server health without player IP addresses or private paths.
- **Expandable:** deeper physics, block, network, script, and mod profilers can be added as isolated opt-in modules.

## Included Features

- Simulation-speed tracking.
- Torch process CPU, working set, managed memory, and thread tracking.
- Online player, entity, grid, static/mobile grid, and total-block counts.
- Top-grid pressure ranking using block count, reflected PCU, and motion.
- `0-100` health score: Healthy, Watch, Degraded, or Critical.
- Configurable warning and critical thresholds.
- Consecutive-sample incident confirmation and alert cooldowns to prevent spam.
- Daily CSV history and `Snapshots/latest.json`.
- On-demand rolling CSV exports.
- Local `Incidents.log` and `Diagnostics.log`.
- Optional Discord incident embeds and webhook test.
- Runtime start/stop, interval change, and config reload.

## Installation

1. Back up your Torch plugin folder and world before testing any alpha plugin.
2. Install `TROA-ProfilerPlus-v1.0.0-alpha.1.zip` through Torch's plugin manager.
3. Restart Torch so the new command modules are registered.
4. TROA Profiler+ creates `TROA-ProfilerPlus.cfg` and `TROA-ProfilerPlusData` in its plugin storage location.
5. Run `!profilerplusadmin status` as a Torch administrator.
6. Wait at least one sampling interval, then run `!profilerplus status` and `!profilerplus topgrids 10`.

## Commands

Commands run through Torch. They can be entered in Space Engineers chat by users with the required promote level or through a compatible Torch command relay.

### Moderator Diagnostics

| Command | Purpose |
|---|---|
| `!profilerplus help` | Shows all normal diagnostic commands. |
| `!profilerplus status` | Shows the newest health sample. |
| `!profilerplus snapshot` | Captures a fresh sample immediately. |
| `!profilerplus report <minutes>` | Summarizes simulation speed, CPU, memory, and health over a rolling window. |
| `!profilerplus topgrids <count>` | Lists the highest-pressure grids with blocks, reflected PCU, score, and entity ID. |
| `!profilerplus incidents <count>` | Shows recent confirmed threshold incidents from the current plugin session. |

### Administrator Controls

| Command | Purpose |
|---|---|
| `!profilerplus export <minutes>` | Exports rolling history to a timestamped CSV. |
| `!profilerplus data` | Shows the resolved data directory. |
| `!profilerplusadmin help` | Shows service/configuration commands. |
| `!profilerplusadmin status` | Shows version, runtime state, interval, history, and webhook status. |
| `!profilerplusadmin start` | Starts runtime sampling when `Enabled` is true. |
| `!profilerplusadmin stop` | Pauses sampling without changing the config. |
| `!profilerplusadmin reload` | Validates and reloads `TROA-ProfilerPlus.cfg`. |
| `!profilerplusadmin interval <seconds>` | Sets and saves a `2-300` second interval, then restarts sampling. |
| `!profilerplusadmin webhook status` | Validates Discord alert settings without posting. |
| `!profilerplusadmin webhook test` | Sends a test Discord embed and reports the HTTP result. |

## Configuration

Every XML element has an opening and closing tag. Edit the generated config, save it, then run `!profilerplusadmin reload`.

| Setting | Default | Purpose |
|---|---:|---|
| `Enabled` | `true` | Master sampling switch. |
| `SampleIntervalSeconds` | `10` | Sampling interval; allowed range `2-300`. |
| `HistoryMinutes` | `120` | Rolling in-memory retention; allowed range `5-10080`. |
| `TopGridCount` | `10` | Grid metrics retained per snapshot; allowed range `1-50`. |
| `MinimumTopGridBlocks` | `1` | Excludes smaller grids from top-grid ranking. |
| `SlowSimulationThreshold` | `0.85` | Opens degraded health when simulation remains below this value. |
| `CriticalSimulationThreshold` | `0.60` | Marks critically slow simulation. |
| `CpuWarningPercent` | `85` | Torch process CPU warning threshold. |
| `MemoryWarningMb` | `12000` | Torch process working-set warning threshold. |
| `ConsecutiveAlertSamples` | `3` | Unhealthy samples required before creating an incident. |
| `AlertCooldownMinutes` | `15` | Minimum time between repeated Discord incidents. |
| `EnableCsvHistory` | `true` | Writes daily rolling history files. |
| `EnableJsonSnapshots` | `true` | Writes `Snapshots/latest.json`. |
| `DataDirectory` | blank | Blank uses `TROA-ProfilerPlusData`; full cross-platform paths are supported. |
| `EnableDiscordWebhook` | `false` | Enables incident embeds. |
| `DiscordWebhookUrl` | blank | Complete HTTPS Discord webhook URL; keep private. |
| `DiscordWebhookName` | `TROA Profiler+` | Discord sender display name. |

## Data Layout

```text
TROA-ProfilerPlusData/
  Diagnostics.log
  History-YYYYMMDD.csv
  Incidents.log
  Exports/
    ProfilerPlus-YYYYMMDD-HHMMSS.csv
  Snapshots/
    latest.json
```

Files are operational diagnostics. They may reveal grid names, entity IDs, server capacity, and timing information; keep them private unless sanitized.

## Health Score

The score begins at `100` and applies penalties for sustained simulation slowdown, high Torch-process CPU, and high working-set memory. It is an operator summary, not proof that a specific grid or mod caused lag. Use `topgrids`, rolling reports, exports, logs, and controlled before/after tests together.

## Performance and Safety

- The alpha sampler does not modify grids, players, factions, ownership, or world saves.
- It does not install WPF controls or require a desktop session.
- It does not enable broad runtime game-method patches.
- Entity/grid collection runs on Torch's game thread; file writing and Discord delivery do not alter world state.
- A shorter interval produces more detail but increases collection and disk-write frequency. Start with `10` seconds on production servers.

## Alpha Limitations

- Reflected grid PCU may report `0` on Keen builds where that internal property changes; block count and motion remain available.
- Online player count uses a guarded runtime API lookup and may show `0` if Keen changes that method.
- The current release identifies pressure correlation, not per-method CPU attribution.
- Incident history is retained in memory for commands and persisted as a local log; after restart, use the log for older incidents.

## Planned Expansion

- Adaptive baselines and anomaly detection.
- Automatic lag incident support bundles.
- Opt-in block-type, programmable-block, session-component, physics-cluster, network, and mod profiling.
- Save and cleanup-cycle correlation.
- Prometheus/OpenTelemetry output for Grafana.
- Cross-server fleet health and regression comparison.

## Support

When reporting an issue, include the plugin version, exact command, Torch log lines, `Diagnostics.log`, a sanitized config, and the relevant CSV/JSON window. Remove webhook URLs, private paths, and sensitive player/server information.

TROA Profiler+ is a new TROA project. It is not a renamed or repackaged release of the legacy Profiler plugin.
