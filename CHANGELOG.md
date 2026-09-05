# TROA Profiler+ Changelog

## v1.0.0-alpha.8

### Physics

- Rebuilds grid physics pressure as a real composite of Havok load drivers (moving mass, angular velocity, joints/constraints, subgrids, awake state) instead of the previous velocity-only score that read ~0 on typical servers.
- Reads physics values straight from the real Space Engineers physics body (`MyGridPhysics`) as strongly-typed members — `Speed`, `Mass`, and `IsActive` — so they match what the game simulates in-game.
- Adds a `!profilerplus physics` command, a Physics Pressure gauge on the status panel, `troa_profiler_active_physics_grids` / `_moving_mass_kg` / `_high_angular_velocity_grids` Prometheus metrics, Grafana panels, and a polished Physics Hotspots Discord embed with parity to the other cards.

### Discord

- Embeds gain a server author bar, unicode sparkline trends, vs-baseline deltas, and owner/faction/GPS detail; correct JSON escaping (all control characters) prevents malformed payloads from unusual grid names.
- Full webhook coverage: `webhook physics`, `webhook timeline`, `webhook overhead`, and a combined `webhook digest`, all preserving mention hygiene, retry/429/timeout, payload bounds, and never logging the webhook URL.

### Reliability and performance

- Caches hot-path reflection (resolve members once per type) to reduce the profiler's own CPU/GC footprint and protect SimSpeed.
- Isolates each persistence writer so a transient disk error no longer aborts a sample or suppresses alerting.

### Diagnostics

- Replaces the never-decaying cumulative baseline with an exponentially-weighted adaptive baseline that tracks recent-normal, plus opt-in deviation alerts when SimSpeed falls below the learned baseline.

## v1.0.0-alpha.7

- Replaces loose runtime-name substring matching with exact Keen block-definition classification for mechanical, script, automation, and AI block families.
- Splits the combined mechanical count into rotors/hinges, pistons, suspensions, wheels, ship connectors, and landing gear.
- Updates incident evidence to show the per-family breakdown instead of presenting one ambiguous combined number.
- Prevents conveyor connectors from being counted as ship connectors and prevents air vents or other `Ai` substring matches from being counted as AI blocks.
- Adds deterministic classification tests against representative Keen object-builder types and misleading near-match names.
- Includes the alpha.6 grid-query fixes for ranks, full names, quoted names, entity IDs, and detailed output suffixes.

## v1.0.0-alpha.6

### Fixed

- Fixes `grid`, `why`, and administrator Discord grid-report commands only reading the first word of grid names.
- Adds full grid-name and quoted grid-name parsing through Torch's complete raw command arguments.
- Adds top-grid rank lookup, so `!profilerplus grid 1` inspects rank `#1` from `topgrids`.
- Keeps exact entity-ID lookup ahead of rank lookup to avoid ambiguity.
- Clarifies that block count and PCU are displayed metrics, not valid grid identifiers.

### Commands

- `!profilerplus grid 1`
- `!profilerplus grid 597 Delta Outpost`
- `!profilerplus grid "597 Delta Outpost" detailed`
- `!profilerplus grid 134343396458477310`
- `!profilerplus why 597 Delta Outpost`

## v1.0.0-alpha.5

### Added

- Adds configurable `Detailed` and `Compact` in-game command layouts with gauges from 5 to 20 segments.
- Adds persistent incident acknowledgement, administrator notes, manual escalation, and configurable automatic escalation for unacknowledged incidents.
- Adds save, cleanup, update, and configuration timeline markers with automatic pre-cleanup snapshots for configured Essentials schedules.
- Adds automatic startup regression markers when the loaded plugin/mod assembly inventory changes.
- Adds incident workflow and operational-marker Prometheus metrics.
- Adds an import-ready Grafana dashboard with health, SimSpeed, CPU, grid pressure, incident, timeline, and marker panels.
- Adds `incident ack`, `incident note`, `incident escalate`, `marker`, and `markers` administrator commands.
- Packages the Grafana dashboard and fully populated example configuration inside the Torch plugin ZIP.

### Reliability

- Records incident workflow actions in `Incidents.log` and incident JSON archives.
- Creates named pre-change snapshots for operational markers so administrators can compare before/after conditions.
- Keeps Essentials support schedule-based and optional, with no hard plugin dependency or invasive runtime patch.
- Keeps regression markers correlation-based; assembly inventory changes do not claim exact mod CPU attribution.

## v1.0.0-alpha.4

### Added

- Adds a consistent, polished in-game command presentation system for player-facing moderator diagnostics and administrator controls.
- Adds readable Unicode gauges for health, SimSpeed, CPU, grid pressure, entity pressure, confidence, world health, and diagnostic score breakdowns.
- Adds visual delta gauges for live comparisons, named snapshot comparisons, and controlled experiments.
- Adds structured headers, dividers, ranked results, status indicators, metric rows, command menus, and safety footers throughout command output.
- Keeps the same headless, command-driven design; no client mod or graphical UI is required.

### Coverage

- Applies the presentation system to server status, reports, grids, entities, players, world health, timelines, snapshots, comparisons, incidents, profiler overhead, Discord queue actions, support bundles, and experiments.
- Adds deterministic command-presentation tests for normalized gauges, unavailable values, delta direction, headers, and Unicode rendering.

## v1.0.0-alpha.3

- Adds complete reusable Discord gauges with a working `DiscordIncludeProgressBars` switch and `N/A` handling.
- Adds dedicated server-health, grid, player-associated, world-health, top-pressure, scheduled, incident, recovery, startup, and shutdown embeds.
- Adds full sampled-grid indexing, owner/faction/Steam-ID association where exposed, real physical-group subgrid lookup, and rotating bounded detail collection.
- Adds Physics, Block Update, PCU, Script, Mechanical, Production, Mining, and Automation pressure subcategories.
- Adds player-associated workload diagnostics without treating ownership as proof of causation.
- Adds non-destructive world-health evidence, data-quality labels, historical change timelines, multi-restart baselines, named snapshots, and arbitrary named-snapshot comparison.
- Adds incident flight records, automatic incident support bundles, manual sanitized support bundles, Prometheus text output, and optional shared fleet summaries.
- Adds adaptive detailed-analysis intervals, collector rotation, self-budget reduction, persistent-condition reminders, and controlled before/after experiment mode.
- Adds deterministic tests for gauges, scoring, confidence, configuration bounds, and XML configuration round-tripping.

## v1.0.0-alpha.2

- Adds explainable `0-100` grid-pressure scoring with centralized configurable thresholds and per-factor evidence.
- Adds incident lifecycle tracking, contributor correlation, confidence classifications, recovery detection, and retained incident JSON archives.
- Adds floating-object, character, voxel/planet entity, total PCU, active tool, production, mechanical, script, and automation observations where exposed.
- Adds historical baselines, two-snapshot comparisons, bounded file retention, expanded CSV/JSON output, and profiler self-overhead measurements.
- Adds queued Discord delivery, specialized optional webhooks, retries, HTTP 429 handling, backoff, safe mentions, payload limits, status colors, startup/shutdown, recovery, incident, and health embeds.
- Adds `grid`, `why`, `compare`, `baseline`, `entities`, and `overhead` diagnostics plus a manual Discord health report.
- Keeps estimates labeled as estimates and explicitly separates correlation from proven causation.

## v1.0.0-alpha.1

- Introduces a completely new headless Torch profiler architecture.
- Adds rolling server health sampling and a `0-100` health score.
- Adds top-grid pressure diagnostics, reports, incidents, CSV history, JSON snapshots, and manual exports.
- Adds optional Discord incident embeds with validation and test commands.
- Adds moderator diagnostics and administrator lifecycle/configuration commands.
- Supports Windows and Linux-hosted AMP/Wine paths without WPF or client dependencies.
