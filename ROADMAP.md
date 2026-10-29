# TROA Profiler+ Public Roadmap

TROA Profiler+ is a read-only Torch diagnostics plugin. This roadmap describes intended priorities, not a release promise. Metrics must remain explicit about what was measured, derived, estimated, or unavailable; correlation is not proof of cause.

## Current baseline

The current documented build is v1.0.0-alpha.21. Profiler+ samples server health, explains estimated grid/entity/physics-cluster pressure, records incidents and operational markers, and provides local exports, optional Discord panels, and sanitized fleet summaries. It does not modify grids, players, ownership, physics, saves, or server configuration.

## Priorities

1. **Metric evidence dictionary:** document each metric's source, classification, sample interval, known overhead, and safe interpretation. Keep unavailable attribution explicit; do not claim per-grid or per-mod CPU causation.
2. **Data quality and overhead:** expose sample age, skipped samples, collector duration, and source availability. Measure collection cost at configured sample rates and large entity counts; provide a bounded retention and storage budget.
3. **Actionable incident summaries:** align SimSpeed, grid-pressure trends, and operational markers in one time window. Tie each recommendation to its supporting evidence and one reversible follow-up check.
4. **Physics timing only behind a validated opt-in:** keep real timing separate from proximity-based estimates. Disable the probe when the game API changes or measured overhead exceeds its accepted budget.
5. **Operator integrations:** document metric-schema compatibility and provide versioned Grafana examples. Any exporter must be opt-in, bounded, and privacy-redacted.

## Safety and release boundary

Profiler+ remains read-only. Network endpoints and sensitive client details remain local-only and must never appear in Discord output or shared fleet data. New measurements are not described as supported until their accuracy, overhead, privacy, and compatibility are validated.

This roadmap is a product direction document; it makes no release-date commitments.

## 2026-10-28 stability candidate

v1.0.0-alpha.26 is available under releases/ with version-specific notes. Live server validation is pending.
