# TROA Profiler+

TROA Profiler+ is a headless Torch performance-diagnostics plugin for Space Engineers dedicated servers. It is operated through Torch commands, XML configuration, local exports, Grafana-compatible files, and optional Discord webhooks. It does not modify grids, players, ownership, physics, saves, or server configuration.

## Current public status

The current documented build is **v1.0.0-alpha.18**. This repository intentionally contains only public operator documentation, a credential-free configuration example, and GitHub issue metadata. Source code and unverified binaries are not published here.

## What it provides

- Interval sampling of SimSpeed, process resources, entity/grid/block counts, and exposed world state.
- Explainable estimated grid, entity, and physics-cluster pressure; estimates are always labelled and never treated as proof of causation.
- Adaptive baseline deviation evidence, incident flight records, support bundles, operational save/cleanup/update markers, and timeline correlation.
- Opt-in block-category pressure evidence, aggregate privacy-safe network state, and loaded component inventory.
- Local Prometheus text output, OpenTelemetry-compatible JSON snapshots, and sanitized shared-directory fleet summaries.
- Discord embeds and an opt-in command webhook mirror. Webhook URLs are never displayed or logged.
- Named snapshots, single-sample comparison, and adjacent rolling regression-window comparison.

## Installation

When a validated release ZIP is supplied, stop Torch, place the ZIP in Torch’s plugin folder, start Torch, load the world, wait one sample interval, then run `!profilerplus status`. Edit the generated `TROA-ProfilerPlus.cfg` in plugin storage and run `!profilerplusadmin reload` to apply safe configuration changes.

Copy [TROA-ProfilerPlus.cfg.example](TROA-ProfilerPlus.cfg.example) only as a starting point. Keep real webhook URLs private.

## Common commands

See the complete [command reference](COMMANDS.md) for every moderator and administrator command, arguments, permissions, and Discord controls.

- `!profilerplus status` — current server-health panel.
- `!profilerplus report 60` — rolling report.
- `!profilerplus topgrids 10`, `grid <rank|name|id>`, `why <grid>` — estimated grid-pressure evidence.
- `!profilerplus physics 5` and `physics inspect <index>` — estimated proximity-based physics clusters. Per-cluster milliseconds remain estimates until the optional timing probe is validated.
- `!profilerplus network` — opt-in aggregate replication state only; no IPs, endpoints, packet contents, or player identities.
- `!profilerplus components` — opt-in loaded assembly inventory for update correlation; not CPU attribution.
- `!profilerplus snapshot save <name>` and `compare snapshots <first> <second>` — named point-in-time comparison.
- `!profilerplus compare windows 30 30` — compares the preceding 30-minute window with the most recent 30-minute window, ranks the largest average changes, and flags unfavorable movement as a regression. It is correlation, not proof that a mod or config caused the change.
- `!profilerplus export 60` — local CSV export.
- `!profilerplusadmin webhook mirror on` — mirror command panels to Discord when webhooks are configured.
- `!profilerplusadmin network clients [count]` — explicit local-only sensitive endpoint list; it is intentionally never mirrored to Discord or written to data files.
- `!profilerplusadmin supportbundle 60` — create a sanitized local support bundle.

## Discord and privacy

Enable `EnableDiscordWebhook`, configure a private webhook URL, reload, then use `!profilerplusadmin webhook status` and `webhook test`. Set `EnableCommandWebhookMirror` to mirror command panels. Webhook delivery is queued away from the simulation thread with bounded retries and rate-limit handling. No player IP address, endpoint, or packet payload is collected or exported.

## Data and integrations

The plugin writes local history, snapshots, incidents, exports, support bundles, `prometheus.prom`, and optional OpenTelemetry-compatible snapshots. It does not start an HTTP listener. Configure your own textfile collector or ingestion workflow for Prometheus/Grafana. Fleet view uses a shared directory and writes sanitized summaries without credentials.

## Safety and limitations

Profiler+ reports measured values where safe APIs expose them and labels derived or estimated indicators. It does not claim exact per-grid CPU, per-cluster physics time, mod CPU, script CPU, conveyor CPU, or causation. The optional real physics timing probe is disabled by default and fails closed after incompatible game updates.

For release-specific changes, see [CHANGELOG.md](CHANGELOG.md). For support, use the issue template and include redacted logs or a sanitized support bundle.