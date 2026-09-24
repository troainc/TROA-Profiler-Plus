# TROA Profiler+ public changelog

## v1.0.0-alpha.19

### Reusable operational command references

- Adds local administrator-managed saved command lines: command save, command list, command show, and command remove.
- Saved lines are bounded, persist in local configuration, and are never executed automatically; normal Torch permissions still apply when an operator reuses one.
- Adds !profilerplusadmin webhook command <name> to post a chosen copy-ready reference as a Discord embed.

### Command and Discord coverage

- Adds !profilerplus tick to the in-game command menu, showing derived TPS from measured SimSpeed without claiming a direct engine tick counter.
- Adds direct !profilerplusadmin webhook tick and !profilerplusadmin webhook why <rank|name|entityId> embeds.
- Expands COMMANDS.md with all command families, privacy boundaries, and ready-to-save grid, player, entity, physics, and tick workflows.


## v1.0.0-alpha.18

### Regression comparison

- Adds `!profilerplus compare windows <beforeMinutes> <afterMinutes>`.
- Splits retained history into adjacent before/after windows, compares window averages for SimSpeed, CPU, memory, health, entity count, grid count, and block count, then ranks the three largest percentage changes.
- Flags unfavorable movement as a regression while explicitly stating that correlation does not prove a mod, configuration, or player caused the change.
- Uses the existing opt-in command-webhook mirror, so the same sanitized command panel can be delivered to Discord.

### Network and fleet command delivery

- !profilerplus network and !profilerplus fleet now return readable setup/status panels, including when instrumentation or shared summaries are not ready; normal command-webhook mirroring can deliver those panels.
- Adds dedicated !profilerplusadmin webhook network and !profilerplusadmin webhook fleet embeds. The network embed remains aggregate-only and excludes endpoint/IP data.

### Public distribution cleanup

- Public repository is documentation/configuration only; private implementation details and unvalidated binaries are excluded.
- Removed the obsolete alpha.8 package instead of presenting it as current.

## v1.0.0-alpha.17

- Adds dedicated sanitized Discord embeds for fleet, aggregate network health, and component inventory.
- Keeps real per-cluster physics timing disabled pending guarded runtime validation.

## v1.0.0-alpha.16

- Adds opt-in local OpenTelemetry-compatible metric snapshots alongside local Prometheus text output; no listener or upload is opened.

## v1.0.0-alpha.15

- Adds sanitized shared-directory fleet summaries for multiple server instances; no central endpoint or credentials are required.

## v1.0.0-alpha.14

- Adds opt-in loaded assembly inventory for update correlation. It is not mod/session-component CPU attribution.

## v1.0.0-alpha.13

- Adds opt-in aggregate replication-state visibility. Unsupported counters remain unavailable; player IPs, endpoints, packet contents, rates, and identities are not collected.

## v1.0.0-alpha.12

- Adds opt-in bounded block-category pressure evidence for programmable/script, production, weapons, exact Keen AI, and conveyor blocks. It is categorization, not per-block CPU measurement.

## v1.0.0-alpha.11

- Adds sanitized lag incident bundles with before/during/after context, top grids, online-player count, and operational save/cleanup/update markers.

## v1.0.0-alpha.10

- Adds opt-in UTC day/hour adaptive baseline learning and deviation evidence.

## v1.0.0-alpha.9

- Adds estimated physics-cluster commands, inspect, administrator teleport, universal command-webhook mirroring, and expanded Discord report embeds.