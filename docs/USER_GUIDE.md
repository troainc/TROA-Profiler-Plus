# TROA Profiler+ — operator guide

Profiler+ samples and summarizes Torch server health. It is diagnostic and non-destructive: it observes server/process/world pressure and reports findings; it does not delete grids or tune the game automatically. Install the versioned release ZIP in Torch, start once, inspect the generated config, and verify the loaded version with the plugin status command. The public repository contains documentation and approved releases, not source code.

## Sampling and health reports

The sampler records available process and world indicators such as simulation speed, CPU, working set/managed memory, players, entities, grids, blocks, and optional aggregate network/component observations. A health score and state summarize the sample. Historical windows, baselines, snapshots, comparisons, rolling reports, and a timeline help determine whether a change is sustained or transient. Configure sample interval, retention, diagnostics, and optional collectors conservatively; extra detail can increase collection cost.

Grid and player tools associate observed workload with grids, subgrids, ownership, and sampled activity where the running game APIs expose it. Pressure scores are diagnostic estimates and explanations, not a direct measurement of every game's physics cost. Physics clusters and tick-rate views are derived indicators; use raw SimSpeed and repeated samples to validate an interpretation.

## Incidents and investigation workflow

Profiler+ can detect repeated unhealthy samples, open/update/recover incidents, and retain local incident context. Operators can acknowledge incidents, add notes, escalate them, and record operational markers such as a mod update or cleanup. A useful investigation sequence is: check current status, review the rolling report and timeline, inspect top grids/entities/players, compare snapshots or windows, record the operational change, then observe whether the signal recovers. Pre/post experiments and support bundles help make support cases reproducible.

## Commands, exports, and integrations

Use `!profiler` commands for status, snapshots, reports, grids, players, incidents, world health, baselines, comparisons, timelines, exports, and data location. Admin commands under the documented admin root control sampling, reload, interval, and Discord output. The complete syntax is maintained by the plugin's command/help output and the release documentation. CSV/JSON/Prometheus outputs and Grafana examples allow external analysis. Optional shared fleet summaries must use an owner-controlled shared folder and sanitized data.

Discord delivery is optional and queued; configure the plugin's own webhook and test it. Available embeds include health, top pressure, grids, players, world/entity diagnostics, incidents, timelines, comparisons, baselines, network, physics, fleet, resource reports, and combined digests where the installed release supports them. Monitor+ does not own these events or require duplicate webhook routes.

## Limits and safe use

Profiler+ does not prove a root cause by itself. Distinguish measured values from derived or estimated results and correlate against server logs, mod changes, player reports, and repeat samples. Keep client/network-identifying data disabled unless you explicitly need it and have reviewed your privacy policy. Collection, exports, and support bundles should be shared only after checking their contents for player or server-sensitive information. Verify the installed release before using settings or commands introduced in later alphas.
