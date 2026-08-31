# TROA Profiler+

TROA Profiler+ is a new headless Torch performance-intelligence plugin for Space Engineers dedicated servers. It was designed from scratch for production administration and does not include the legacy Profiler source, command names, WPF interface, patch framework, Jenkins files, dependencies, or branding.

## Current Release

- Version: `v1.0.0-alpha.2`
- Package: `TROA-ProfilerPlus-v1.0.0-alpha.2.zip`
- SHA-256: `40B422822CFEA7FCBCC1F7C8E1ACDE0F5345DDB131B63638D4AB2A9D1113D3CC`
- Runtime: Torch / .NET Framework 4.8
- Hosting: Windows and Linux-hosted AMP/Wine servers
- UI: none; all operation is command-, config-, and file-based

## Implemented Features

- Lightweight interval sampling without patching the Space Engineers game loop.
- Current simulation speed, process CPU, working set, managed memory, process threads, players, entities, grids, and blocks.
- Explainable estimated grid pressure using motion, mechanical systems, scripts, automation, active tools, production, blocks, and PCU where available.
- A readable `0-100` health score with Healthy, Warning, High Load, and Critical states.
- Rolling in-memory history and configurable retention.
- Debounced incidents that require consecutive unhealthy samples and respect alert cooldowns.
- Daily CSV history, latest JSON snapshot, manual rolling CSV exports, local incident log, and diagnostic log.
- Incident start, update, recovery, correlation findings, confidence classifications, and retained incident archives.
- Floating-object, character, voxel/planet entity, total PCU, active tool, production, mechanical, script, and automation observations where current APIs expose them.
- Optional queued Discord incident, recovery, startup, shutdown, scheduled health, and manual health embeds with retries and HTTP 429 handling.
- Historical baseline, two-snapshot comparison, bounded retention, and profiler self-overhead diagnostics.
- Runtime start, stop, interval adjustment, and safe XML config reload.
- Moderator read commands and administrator control/export commands.
- Path-safe storage for Windows and AMP/Wine hosts.

## Commands

### Moderator

- `!profilerplus help`
- `!profilerplus status`
- `!profilerplus snapshot`
- `!profilerplus report <minutes>`
- `!profilerplus topgrids <count>`
- `!profilerplus grid <name-or-id> [detailed]`
- `!profilerplus why <name-or-id>`
- `!profilerplus compare`
- `!profilerplus baseline`
- `!profilerplus entities`
- `!profilerplus incidents <count>`

### Administrator

- `!profilerplus export <minutes>`
- `!profilerplus data`
- `!profilerplus overhead`
- `!profilerplusadmin help`
- `!profilerplusadmin status`
- `!profilerplusadmin start`
- `!profilerplusadmin stop`
- `!profilerplusadmin reload`
- `!profilerplusadmin interval <seconds>`
- `!profilerplusadmin webhook status`
- `!profilerplusadmin webhook test`
- `!profilerplusadmin webhook health`

## Build

```powershell
dotnet build .\TROA-ProfilerPlus.csproj -c Release -p:TorchDirectory="C:\path\to\Torch" -p:SpaceEngineersBin64="C:\path\to\SpaceEngineers\Bin64"
```

The build target creates a Torch-ready ZIP containing only `manifest.xml` and `TROA-ProfilerPlus.dll`.

## Configuration Reference

| Setting | Default | Purpose |
|---|---:|---|
| `Enabled` | `true` | Master sampling switch. |
| `SampleIntervalSeconds` | `10` | Interval from `2-300` seconds. |
| `HistoryMinutes` | `120` | Rolling retention from `5-10080` minutes. |
| `TopGridCount` | `10` | Grid records retained per sample, from `1-50`. |
| `MinimumTopGridBlocks` | `1` | Minimum grid size included in pressure ranking. |
| `SlowSimulationThreshold` | `0.85` | Degraded simulation threshold. |
| `CriticalSimulationThreshold` | `0.60` | Critical simulation threshold. |
| `HealthySimulationThreshold` | `0.95` | Healthy SimSpeed boundary. |
| `HighLoadSimulationThreshold` | `0.80` | High-load SimSpeed boundary. |
| `CpuWarningPercent` | `85` | Torch process CPU warning threshold. |
| `MemoryWarningMb` | `12000` | Torch process working-set warning threshold. |
| `ConsecutiveAlertSamples` | `3` | Unhealthy samples required to confirm an incident. |
| `AlertCooldownMinutes` | `15` | Repeat-alert cooldown. |
| `PersistentAlertReminderMinutes` | `15` | Reserved reminder interval for unresolved incidents. |
| `GridPressureModerate` | `40` | Moderate estimated grid-pressure boundary. |
| `GridPressureHigh` | `70` | High estimated grid-pressure boundary. |
| `GridPressureCritical` | `90` | Critical estimated grid-pressure boundary. |
| `GridBlocksHigh` | `20000` | Block normalization reference, not a block limit. |
| `GridPcuHigh` | `100000` | PCU normalization reference, not proof of lag. |
| `GridMechanicalBlocksHigh` | `30` | Mechanical normalization reference. |
| `GridSubgridsHigh` | `12` | Subgrid normalization reference; unavailable values remain zero. |
| `GridScriptsHigh` | `10` | Script/automation normalization reference. |
| `GridActiveToolsHigh` | `30` | Active drill/welder/grinder normalization reference. |
| `GridProductionBlocksHigh` | `20` | Active production normalization reference. |
| `GridLinearSpeedHigh` | `50` | Linear-motion normalization reference in m/s. |
| `GridAngularSpeedHigh` | `2` | Angular-motion normalization reference in rad/s. |
| `EnableCsvHistory` | `true` | Writes daily CSV history. |
| `EnableJsonSnapshots` | `true` | Writes the latest JSON snapshot. |
| `DataDirectory` | blank | Blank uses `TROA-ProfilerPlusData`. |
| `RawHistoryRetainDays` | `7` | Daily raw CSV retention. |
| `IncidentRetainDays` | `90` | Incident JSON retention. |
| `SnapshotRetainDays` | `30` | Snapshot JSON retention. |
| `BaselineHours` | `168` | Requested baseline window; alpha baselines use retained in-memory samples. |
| `SlowCollectorWarningMs` | `500` | Logs a profiler self-overhead warning. |
| `EnableDiscordWebhook` | `false` | Enables Discord incident embeds. |
| `DiscordWebhookUrl` | blank | Complete private Discord webhook URL. |
| `DiscordWebhookName` | `TROA Profiler+` | Discord sender display name. |
| `DiscordServerHealthWebhookUrl` | blank | Optional routine-health destination. |
| `DiscordPerformanceAlertWebhookUrl` | blank | Optional incident/recovery destination. |
| `DiscordWorldHealthWebhookUrl` | blank | Reserved world-health destination. |
| `DiscordAdministrativeWebhookUrl` | blank | Optional test/detailed-report destination. |
| `DiscordRecoveryAlertsEnabled` | `true` | Sends recovery transitions. |
| `DiscordIncludeRecommendations` | `true` | Includes diagnostic-first recommendations. |
| `DiscordIncludeConfidence` | `true` | Includes documented confidence indicators. |
| `DiscordIncludeProgressBars` | `true` | Enables normalized progress-bar presentation. |
| `DiscordScheduledReportMinutes` | `0` | `0` disables scheduled health reports. |
| `DiscordMaximumOffenders` | `5` | Maximum contributors shown per embed. |
| `DiscordTimeoutSeconds` | `15` | Reserved network timeout setting. |
| `DiscordRetryCount` | `3` | Retries transient failures and rate limits. |

## How Diagnostics Work

TROA Profiler+ follows **Measure → Correlate → Diagnose → Explain → Recommend → Alert**. Raw samples are collected at the configured interval. Expensive world enumeration occurs only on that interval and never every simulation tick. Process-resource collection, game-state collection, analysis, persistence, and Discord queue depth are measured so the profiler can report its own overhead.

### SimSpeed

- `0.95-1.00`: Healthy by default.
- `0.80-0.949`: Warning by default.
- `0.60-0.799`: High Load by default.
- Below `0.60`: Critical by default.

All boundaries are configurable. An incident requires consecutive unhealthy samples, receives a short unique ID, tracks minimum and average SimSpeed, stores potential contributors, and closes only after recovery. Recovery produces an archive entry and optional Discord embed.

### Grid Pressure

Grid Pressure is an **estimated diagnostic indicator**, not Keen PCU and not proof that a grid caused lag. The score is capped at `100` and is assembled from visible factors. The `!profilerplus why` command lists every awarded component and its evidence. Default maximum contributions are:

- Physics motion: `26` points.
- Mechanical systems: `18` points.
- Connected-subgrid signal: `14` points when measurable.
- Scripts and automation: `14` points.
- Active drills, welders, and grinders: `13` points.
- Active production: `9` points.
- Grid complexity: `10` points.
- PCU signal: `6` points when available.

PCU and block count are only two signals. A large static station can score below a smaller moving mechanical mining grid. Missing values contribute no points rather than fabricated measurements.

### Confidence and Correlation

Confidence considers timing, the size of the SimSpeed change, current pressure, pressure change from the previous sample, observable active systems, and repeated matches when available. Classifications are Low, Moderate, High, and Very High. The current alpha records **potential contributors** and uses cautious language. Ownership, block count, or PCU alone never identifies a player or grid as the cause of lag.

### Entity Pressure

The current collector counts exposed entities and separately identifies floating objects, characters, and voxel/planet entities by their runtime types. Entity Pressure is an estimated world-level indicator. Planet presence is never reported as proof of lag; exact voxel-work milliseconds are not exposed by the supported API surface.

## Discord Setup and Reliability

1. Create a Discord webhook in the destination channel.
2. Set `EnableDiscordWebhook` to `true`.
3. Put the complete credential in `DiscordWebhookUrl`, or use one or more specialized webhook settings.
4. Restart Torch or run `!profilerplusadmin reload`.
5. Run `!profilerplusadmin webhook status` and `!profilerplusadmin webhook test`.

Specialized URLs fall back to `DiscordWebhookUrl`. Discord work is queued away from the simulation thread. Delivery uses timeouts, bounded retry attempts, exponential backoff, `429 Retry-After` handling, payload truncation, disabled mass mentions, and redacted failure messages. Webhook URLs are credentials: never post the config or full URL in logs or support channels.

## Installation

1. Stop the Torch server.
2. Copy `TROA-ProfilerPlus-v1.0.0-alpha.2.zip` into Torch's plugin folder.
3. Start Torch and load the Space Engineers world.
4. Confirm `TROA-ProfilerPlus.cfg` and `TROA-ProfilerPlusData` are created in plugin storage.
5. Run `!profilerplusadmin status`.
6. Wait at least one sample interval, then run `!profilerplus status`, `!profilerplus topgrids 10`, and `!profilerplus overhead`.

Linux-hosted AMP deployments normally run Torch through Wine/Proton-compatible infrastructure. Leave `DataDirectory` blank for the safest portable default. If an absolute path is used, it must be valid from the Torch process's view of the filesystem.

## Data Layout

- `History-YYYYMMDD.csv`: bounded raw sample history.
- `Snapshots/latest.json`: latest structured sample.
- `Incidents.log`: human-readable incident transitions.
- `Incidents/*.json`: recovered incident evidence and findings.
- `Exports/*.csv`: manual rolling exports.
- `Diagnostics.log`: collector, persistence, Discord, and self-overhead warnings.

These files can contain grid names, entity IDs, coordinates, and operational details. Treat them as administrative data and sanitize them before sharing.

## API Limitations

Directly measured where available: process CPU, memory, threads, entity/grid/block counts, grid motion, runtime block categories, active-state indicators, and current SimSpeed. Reflected or guarded values can become unavailable after Keen/Torch updates; unavailable PCU or player-count values display as zero or `N/A` depending on the command.

Estimated rather than directly measured: Grid Pressure, Entity Pressure, Performance Impact, confidence, and contributor correlation. Space Engineers/Torch does not provide reliable exact per-grid physics milliseconds, per-grid CPU time, conveyor milliseconds, script milliseconds, voxel-work milliseconds, or proof that one player/grid caused an incident through the APIs used by this release.

Not yet implemented in alpha.2: destructive repair, automatic cleanup, exact corruption repair, a web UI, a WPF UI, deep log-stream world-corruption parsing, persisted multi-day aggregate baselines across restarts, per-player Steam-ID workload reports, exact mod/session-component CPU attribution, or a public REST endpoint. These remain planned only where they can be implemented without inventing measurements or harming SimSpeed.

## Data Safety

The sampler reads current world/process state and writes profiler data only. It does not alter grids, ownership, physics, players, factions, world saves, or Torch configuration. This alpha intentionally avoids runtime game-method patches until each deeper profiler is independently tested against current Keen builds.

See `ROADMAP.md` for the prioritized feature plan.
