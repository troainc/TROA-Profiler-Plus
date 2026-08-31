# TROA Profiler+ Changelog

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
