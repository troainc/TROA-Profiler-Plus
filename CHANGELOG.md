# TROA Profiler+ Changelog

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
