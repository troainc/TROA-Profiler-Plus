# TROA Profiler+

TROA Profiler+ is a new headless Torch performance-intelligence plugin for Space Engineers dedicated servers. It was designed from scratch for production administration and does not include the legacy Profiler source, command names, WPF interface, patch framework, Jenkins files, dependencies, or branding.

## Current Release

- Version: `v1.0.0-alpha.6`
- Package: `TROA-ProfilerPlus-v1.0.0-alpha.6.zip`
- SHA-256: `B0FF332504CD6104D4044587881F47615537CB8F34CCF9D170FB6909E9AEA808`
- Runtime: Torch / .NET Framework 4.8
- Hosting: Windows and Linux-hosted AMP/Wine servers
- UI: none; all operation is command-, config-, and file-based

## Implemented Features

- Lightweight interval sampling without patching the Space Engineers game loop.
- Current simulation speed, process CPU, working set, managed memory, process threads, players, entities, grids, and blocks.
- Explainable estimated grid pressure using motion, mechanical systems, scripts, automation, active tools, production, blocks, and PCU where available.
- A readable `0-100` health score with Healthy, Warning, High Load, and Critical states.
- Polished in-game panels and Unicode gauges across diagnostic and administrative commands, including visual status, ranking, and before/after changes without requiring Discord.
- Configurable compact/detailed command layouts and 5-20 segment in-game gauges.
- Persistent incident acknowledgement, notes, escalation, operational markers, pre-cleanup snapshots, and update regression markers.
- Import-ready Grafana dashboard using the local Prometheus metrics file.
- Rolling in-memory history and configurable retention.
- Debounced incidents that require consecutive unhealthy samples and respect alert cooldowns.
- Daily CSV history, latest JSON snapshot, manual rolling CSV exports, local incident log, and diagnostic log.
- Incident start, update, recovery, correlation findings, confidence classifications, and retained incident archives.
- Floating-object, character, voxel/planet entity, total PCU, active tool, production, mechanical, script, and automation observations where current APIs expose them.
- Optional queued Discord incident, recovery, startup, shutdown, scheduled health, and manual health embeds with retries and HTTP 429 handling.
- Historical baseline, two-snapshot comparison, bounded retention, and profiler self-overhead diagnostics.
- Persistent multi-restart baselines, named snapshots, arbitrary named comparisons, and a measured/derived/estimated timeline.
- Full sampled-grid indexing with rotating detailed analysis, owner/faction/Steam-ID association, subgrids, and pressure subcategories.
- Player-associated workload and non-destructive world-health diagnostics.
- Incident flight recording, automatic/manual support bundles, Prometheus text output, and optional shared fleet summaries.
- Reusable Discord gauges and dedicated embeds for health, grids, players, world health, top pressure, scheduled reports, incidents, recovery, startup, and shutdown.
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
- `!profilerplus grid <rank|full-name|entity-id> [detailed]`
- `!profilerplus why <rank|full-name|entity-id>`
- `!profilerplus compare`
- `!profilerplus baseline`
- `!profilerplus entities`
- `!profilerplus players <count>`
- `!profilerplus player <name-or-id>`
- `!profilerplus worldhealth`
- `!profilerplus timeline <minutes>`
- `!profilerplus snapshot save <name>`
- `!profilerplus snapshot list`
- `!profilerplus compare snapshots <first> <second>`
- `!profilerplus incidents <count>`

Grid inspection accepts a `topgrids` rank, complete grid name, quoted grid name, or entity ID. Examples: `!profilerplus grid 1`, `!profilerplus grid 597 Delta Outpost`, `!profilerplus grid "597 Delta Outpost" detailed`, and `!profilerplus grid 134343396458477310`. The displayed block count and PCU are metrics, not identifiers.

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
- `!profilerplusadmin webhook top`
- `!profilerplusadmin webhook grid <rank|full-name|entity-id>`
- `!profilerplusadmin webhook player <name-or-id>`
- `!profilerplusadmin webhook world`
- `!profilerplusadmin supportbundle <minutes>`
- `!profilerplusadmin experiment start <name>`
- `!profilerplusadmin experiment end`
- `!profilerplusadmin incident ack <id> [note]`
- `!profilerplusadmin incident note <id> <note>`
- `!profilerplusadmin incident escalate <id> [note]`
- `!profilerplusadmin marker <save|cleanup|update|config> <detail>`
- `!profilerplusadmin markers <count>`

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
| `FloatingObjectsCritical` | `2000` | Entity-pressure normalization reference. |
| `EntitiesCritical` | `10000` | Total-entity pressure normalization reference. |
| `OwnerlessGridWarningCount` | `100` | Informational world-health threshold. |
| `TimelinePressureChange` | `15` | Grid-pressure change required for a timeline event. |
| `MaximumTimelineEventsPerSample` | `25` | Bounds timeline work and storage. |
| `FlightRecorderMinutesBefore` | `10` | History preserved before an incident. |
| `FlightRecorderMinutesAfter` | `5` | Reserved post-recovery recording target. |
| `MaximumDetailedGridsPerSample` | `100` | Rotating detailed-grid budget. |
| `NormalDetailedAnalysisSeconds` | `60` | Normal detailed-collector interval. |
| `IncidentDetailedAnalysisSeconds` | `10` | Faster incident detail interval. |
| `EnablePrometheusExport` | `true` | Writes a local Prometheus text file. |
| `FleetDirectory` | blank | Optional shared cross-server summary directory. |
| `ServerInstanceName` | `Space Engineers Server` | Display/fleet identity. |
| `InGameCommandLayout` | `Detailed` | `Detailed` aligned panels or narrower `Compact` output. |
| `InGameGaugeWidth` | `10` | In-game gauge segments from `5-20`. |
| `EnableIncidentEscalation` | `true` | Escalates unresolved, unacknowledged incidents. |
| `IncidentEscalationMinutes` | `30` | Minutes before automatic escalation. |
| `EnableOperationalCorrelation` | `true` | Enables save/cleanup/update/config timeline markers. |
| `EssentialsCleanupIntervalMinutes` | `0` | Actual Essentials cleanup interval; `0` disables schedule prediction. |
| `PreCleanupProfileMinutes` | `10` | Lead time for automatic pre-cleanup capture. |
| `EnableAutomaticRegressionMarkers` | `true` | Marks loaded assembly inventory changes between startups. |

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

### Discord Gauges

Normalized values use ten-segment gauges such as `████████░░ 82%`. Set `DiscordIncludeProgressBars` to `false` to display numeric `82/100` values instead. Unavailable or non-normalizable measurements display `N/A`. Gauges are available for server health, grid pressure, physics, entities, scripts, mechanical systems, production, mining, automation, PCU when available, performance impact, and confidence.

Use `!profilerplusadmin webhook health`, `top`, `grid`, `player`, or `world` to preview each report. Scheduled reports include average/minimum SimSpeed and peak process resources from the configured interval.

## Grid Command Troubleshooting

Run `!profilerplus topgrids` first to see the current sampled ranking, grid names, and entity IDs. The `#` number is a temporary rank for the latest sample and can change as server pressure changes.

- Use `!profilerplus grid 1` to inspect the current `#1` ranked grid.
- Use `!profilerplus grid 597 Delta Outpost` for an unquoted complete name.
- Use `!profilerplus grid "597 Delta Outpost" detailed` for a quoted name and expanded evidence.
- Use `!profilerplus grid 134343396458477310` for an exact entity ID.
- Use the same rank, name, or entity-ID formats with `why` and `profilerplusadmin webhook grid`.

The values shown as `Blocks / PCU / ID` are three different fields. Only the final `ID` value is the persistent grid entity ID. Block count and PCU cannot be used as grid identifiers. When duplicate names exist, use the entity ID to select the exact grid.

## Installation

1. Stop the Torch server.
2. Copy `TROA-ProfilerPlus-v1.0.0-alpha.6.zip` into Torch's plugin folder.
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
- `Timeline.log`: measured, derived, and estimated change events.
- `Baseline.state`: bounded multi-restart aggregate baseline state.
- `Incidents/*-flight.csv`: pre-incident flight-recorder window.
- `SupportBundles/*.zip`: incident or manual sanitized diagnostic packages.
- `prometheus.prom`: local metrics for Prometheus-compatible collectors.
- `OperationalMarkers.log`: save, cleanup, update, and configuration correlation points.
- `RuntimeFingerprint.state`: private hash used to detect loaded assembly inventory changes between starts.

These files can contain grid names, entity IDs, coordinates, and operational details. Treat them as administrative data and sanitize them before sharing.

## API Limitations

Directly measured where available: process CPU, memory, threads, entity/grid/block counts, grid motion, runtime block categories, active-state indicators, and current SimSpeed. Reflected or guarded values can become unavailable after Keen/Torch updates; unavailable PCU or player-count values display as zero or `N/A` depending on the command.

Estimated rather than directly measured: Grid Pressure, Entity Pressure, Performance Impact, confidence, and contributor correlation. Space Engineers/Torch does not provide reliable exact per-grid physics milliseconds, per-grid CPU time, conveyor milliseconds, script milliseconds, voxel-work milliseconds, or proof that one player/grid caused an incident through the APIs used by this release.

Intentionally not implemented: destructive repair, automatic entity/grid deletion, a plugin UI, WPF, an embedded public web server, or claims of exact causation. Exact per-grid CPU/physics/script/conveyor/voxel milliseconds and exact mod/session-component CPU attribution remain unavailable through the safe APIs used here. World health therefore reports evidence visible to the collector and never performs repairs.

## Advanced Operations

### Incident Flight Recorder

Confirmed degradation starts a bounded incident record containing recent samples, SimSpeed, CPU, memory, world counts, floating objects, and pressure categories. Recovery archives findings and builds an incident support ZIP. Persistent incidents use state reminders rather than posting every sample.

### Controlled Experiment Mode

`experiment start` captures a before state. Make one controlled administrative change, wait for additional samples, then use `experiment end`. TROA Profiler+ reports the before/after difference but never disables grids or blocks automatically.

### Prometheus and Fleet Summaries

`prometheus.prom` is written atomically as a local text exposition file and does not open a network listener. `FleetDirectory` can point multiple server instances at a shared folder; each server writes one small summary JSON for external dashboards or fleet comparisons.

Import `Grafana/TROA-ProfilerPlus-dashboard.json` into Grafana after configuring a Prometheus-compatible collector to scrape or ingest `prometheus.prom`. The supplied dashboard includes server-health, SimSpeed, CPU, grid-pressure, incident-workflow, and operational-marker panels.

### Incident Workflow and Operational Correlation

Administrators can acknowledge incidents, append audited notes, or escalate them by incident ID. Unacknowledged incidents automatically escalate after `IncidentEscalationMinutes` when enabled. Workflow changes are written to `Incidents.log` and incident JSON.

Use `marker save`, `marker cleanup`, `marker update`, or `marker config` immediately before operational changes. Each marker creates a named pre-change snapshot. When `EssentialsCleanupIntervalMinutes` matches the server's real Essentials schedule, TROA Profiler+ automatically creates a pre-cleanup marker and snapshot. This is schedule correlation, not a runtime dependency or invasive Essentials patch.

At startup, the plugin hashes the loaded managed assembly name/version inventory. A changed fingerprint creates an `UPDATE` marker, helping administrators compare performance before and after plugin or mod changes without claiming exact component-level CPU attribution.

### Profiler Protection

Detailed block inspection runs on a slower configurable cadence, rotates through a bounded number of grids, accelerates during incidents, and halves its detail budget after an over-budget collection. The `overhead` command reports process collection, game collection, analysis, persistence, skipped samples, and Discord queue depth.

## Testing

The source repository includes a dependency-free deterministic test harness covering gauge rendering, unavailable values, score caps and explanations, confidence classifications, configuration bounds, and XML configuration round-tripping. Run `dotnet run --project .\Tests\TROA-ProfilerPlus.Tests.csproj -c Release`.

## Data Safety

The sampler reads current world/process state and writes profiler data only. It does not alter grids, ownership, physics, players, factions, world saves, or Torch configuration. This alpha intentionally avoids runtime game-method patches until each deeper profiler is independently tested against current Keen builds.

See `ROADMAP.md` for the prioritized feature plan.
