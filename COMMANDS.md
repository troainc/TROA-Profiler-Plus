# TROA Profiler+ command reference

Commands run in in-game chat or the Torch console. `!profilerplus` commands are read-only moderator diagnostics unless noted. `!profilerplusadmin` commands require an administrator promote level. Values described as estimated or correlated are not proof of causation.

## Moderator commands

| Command | Purpose |
|---|---|
| `!profilerplus help` | Show the command menu. |
| `!profilerplus status` | Show current health, SimSpeed, process resources, pressure indicators, and world counts. |
| `!profilerplus snapshot` | Capture and display a fresh sample. |
| `!profilerplus report [minutes]` | Show a rolling health report. |
| `!profilerplus topgrids [count]` | Rank sampled grids by estimated pressure. |
| `!profilerplus grid <rank\|name\|entityId> [detailed]` | Inspect a grid. Quote names with spaces when needed. |
| `!profilerplus why <rank\|name\|entityId>` | Explain the grid-pressure score. |
| `!profilerplus physics [count]` | List estimated proximity-based physics clusters. |
| `!profilerplus physics inspect <index>` | List grids within an estimated physics cluster. |
| `!profilerplus incidents [count]` | List retained profiler incidents. |
| `!profilerplus entities` | Show entity and floating-object pressure. |
| `!profilerplus players [count]` | List player-associated sampled workload. |
| `!profilerplus player <name-or-id>` | Show sampled workload associated with one player. |
| `!profilerplus worldhealth` | Show non-destructive world-health findings. |
| `!profilerplus timeline [minutes]` | Show recent measured, derived, and estimated timeline events. |
| `!profilerplus baseline` | Show the learned baseline summary. |
| `!profilerplus compare` | Compare the two latest samples. |
| `!profilerplus snapshot save <name>` | Save the latest sample as a named snapshot. |
| `!profilerplus snapshot list` | List recent named snapshots. |
| `!profilerplus compare snapshots <first> <second>` | Compare two named snapshots from the current session. |
| `!profilerplus compare windows [beforeMinutes] [afterMinutes]` | Compare adjacent retained before/after windows and rank the largest changes. Example: `compare windows 30 30`. |
| `!profilerplus network` | Show opt-in aggregate replication state. Unsupported values show unavailable; no endpoints, IPs, packet contents, or client identity data is shown. |
| `!profilerplus components` | Show opt-in loaded managed-assembly inventory for update correlation. It is not CPU attribution. |
| `!profilerplus fleet` | Show sanitized shared-directory fleet summaries when fleet view is configured. |
| `!profilerplus overhead` | Show profiler collection overhead and Discord queue depth. |
| `!profilerplus export [minutes]` | Write rolling history to a local CSV export. Administrator permission is required. |
| `!profilerplus data` | Show the local profiler data directory. Administrator permission is required. |

## Administrator service controls

| Command | Purpose |
|---|---|
| `!profilerplusadmin help` | Show administrator controls. |
| `!profilerplusadmin status` | Show service status, configuration, and webhook state. |
| `!profilerplusadmin start` | Start sampling. |
| `!profilerplusadmin stop` | Stop sampling. |
| `!profilerplusadmin reload` | Validate and reload `TROA-ProfilerPlus.cfg`. |
| `!profilerplusadmin interval <seconds>` | Set the sampling interval within the supported range. |
| `!profilerplusadmin physics takeme <index>` | Best-effort teleport to the centre of an estimated physics cluster. |
| `!profilerplusadmin supportbundle [minutes]` | Create a sanitized local support ZIP. Review it before sharing because world metrics may be sensitive. |

## Administrator Discord controls

Configure `EnableDiscordWebhook` and a private webhook URL first. `webhook mirror on` mirrors command panels to Discord; it does not expose webhook credentials.

| Command | Purpose |
|---|---|
| `!profilerplusadmin webhook status` | Show safe webhook configuration/delivery status. |
| `!profilerplusadmin webhook mirror <on\|off>` | Enable or disable the universal command-result mirror. |
| `!profilerplusadmin webhook test` | Queue a test embed. |
| `!profilerplusadmin webhook health` | Queue the current health embed. |
| `!profilerplusadmin webhook top` | Queue the top-pressure embed. |
| `!profilerplusadmin webhook grid <rank\|name\|entityId>` | Queue a grid-analysis embed. |
| `!profilerplusadmin webhook player <name-or-id>` | Queue a player-associated workload embed. |
| `!profilerplusadmin webhook world` | Queue a world-health embed. |
| `!profilerplusadmin webhook report [minutes]` | Queue a rolling-report embed. |
| `!profilerplusadmin webhook incidents [count]` | Queue an incident-archive embed. |
| `!profilerplusadmin webhook entities` | Queue an entity-diagnostics embed. |
| `!profilerplusadmin webhook players [count]` | Queue a player-pressure ranking embed. |
| `!profilerplusadmin webhook timeline [minutes]` | Queue a performance-timeline embed. |
| `!profilerplusadmin webhook baseline` | Queue a learned-baseline embed. |
| `!profilerplusadmin webhook compare` | Queue the latest two-sample comparison embed. |
| `!profilerplusadmin webhook overhead` | Queue a profiler-overhead embed. |
| `!profilerplusadmin webhook physics [count]` | Queue an estimated physics-cluster embed. |
| !profilerplusadmin webhook network | Queue the aggregate network-health embed. It excludes endpoint and IP data. |
| !profilerplusadmin webhook fleet | Queue the sanitized shared-folder fleet-health embed. |
| `!profilerplusadmin webhook digest` | Queue a combined server-digest embed. |

## Administrator incident and correlation controls

| Command | Purpose |
|---|---|
| `!profilerplusadmin experiment start <name>` | Capture a before state for one controlled diagnostic change. |
| `!profilerplusadmin experiment end` | End the experiment and compare its before/after state. |
| `!profilerplusadmin incident ack <id> [note]` | Acknowledge an incident. |
| `!profilerplusadmin incident note <id> <note>` | Append an administrator note to an incident. |
| `!profilerplusadmin incident escalate <id> [note]` | Mark an incident escalated and, when configured, deliver its alert. |
| `!profilerplusadmin marker <save\|cleanup\|update\|config> <detail>` | Record an operational marker and capture a pre-change snapshot when available. |
| `!profilerplusadmin markers [count]` | List recent operational markers. |

## Practical workflow

1. Run `!profilerplus snapshot save Before-Update` before a mod/config change.
2. Apply one change and let comparable samples accumulate.
3. Run `!profilerplus compare windows 30 30` or `compare snapshots Before-Update <after>`.
4. Use `topgrids`, `physics`, `network`, `components`, and `timeline` for evidence.
5. Create a support bundle only after reviewing what you intend to share.