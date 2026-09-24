# TROA Profiler+ command reference

Commands run in in-game chat or the Torch console. `!profilerplus` commands are read-only moderator diagnostics unless noted. `!profilerplusadmin` commands require an administrator promote level. Estimated and correlated values are diagnostic evidence, not proof of causation.

## Webhook delivery

Set `EnableCommandWebhookMirror=true` or run `!profilerplusadmin webhook mirror on` to mirror every ordinary command panel to Discord. Use a dedicated `webhook <report>` command when an operator needs an on-demand embed. Local filesystem paths from `export` and `data` are never sent to Discord; those commands mirror only safe status panels. `network clients` is intentionally local-only because it can expose client IP addresses.

## Reusable command lines

Server owners can save up to 25 named, copy-ready Profiler+ command lines in the local configuration. Saved lines are **never automatically executed**: Torch still performs normal permission checks when an admin copies and runs one. This prevents a saved shortcut from becoming a permissions bypass.

| Command | Purpose |
|---|---|
| `!profilerplusadmin command save <name> <full-command>` | Save a reusable line. It must start with `!profilerplus` or `!profilerplusadmin`. |
| `!profilerplusadmin command list` | List every saved reference. |
| `!profilerplusadmin command show <name>` | Display one copy-ready line. |
| `!profilerplusadmin command remove <name>` | Delete a saved line. |
| `!profilerplusadmin webhook command <name>` | Post a saved line as a copy-ready Discord embed. |

Examples to save:

```text
!profilerplusadmin command save DailyHealth !profilerplus report 60
!profilerplusadmin command save TickNow !profilerplus tick
!profilerplusadmin command save TopGrid !profilerplus grid 1 detailed
!profilerplusadmin command save GridWhy !profilerplus why 1
!profilerplusadmin command save PlayerCheck !profilerplus player "Player Name"
!profilerplusadmin command save Entities !profilerplus entities
!profilerplusadmin command save Physics !profilerplus physics 10
!profilerplusadmin command save Cluster0 !profilerplus physics inspect 0
```

After saving one, use `!profilerplusadmin webhook command TopGrid` to post it to Discord for the admin team to copy. References persist in the local config and can also be maintained in the `SavedCommandReferences` section of `TROA-ProfilerPlus.cfg`.

## Custom resource webhook builder

Use one flexible command when operators want to attach context and request size/time controls to a Discord panel:

```text
!profilerplusadmin webhook resource <type> [selector] [--minutes N] [--count N] [--title "text"] [--note "text"] [--fresh]
```

Supported types: `tick`, `status`, `report`, `topgrids`, `grid`, `why`, `player`, `players`, `entities`, `physics`, `timeline`, `baseline`, `compare`, `incidents`, `network`, `fleet`, and `overhead`.

| Option | Effect |
|---|---|
| `--minutes N` | Retained-history window for report, timeline, and similar time-based resources. |
| `--count N` | Limit listed grids, players, physics clusters, incidents, fleet rows, or timeline events. |
| `--title "text"` | Replace the Discord embed title with an operator label. |
| `--note "text"` | Append operator context to the embed, for example a mod-change or incident note. |
| `--fresh` | Capture one fresh bounded sample before rendering. |

Examples:

```text
!profilerplusadmin webhook resource tick --title "Peak check" --note "After event start" --fresh
!profilerplusadmin webhook resource report --minutes 60 --title "Hourly health" --note "Post-update review"
!profilerplusadmin webhook resource grid 1 --title "Top suspect" --note "Investigate active tools"
!profilerplusadmin webhook resource why 1 --note "Pressure breakdown for ticket 184"
!profilerplusadmin webhook resource player "Player Name" --note "Owner workload review"
!profilerplusadmin webhook resource physics --count 10 --title "Physics hotspots"
!profilerplusadmin webhook resource entities --fresh --note "Cleanup decision"
```

`--ticks` is intentionally rejected: Profiler+ does not patch the Space Engineers game loop or pretend it has a direct tick counter. Use `--minutes N` for retained history or `--fresh` for one current sample.
## Moderator commands

| Command | Purpose |
|---|---|
| `!profilerplus help` | Show the command menu. |
| `!profilerplus tick` | Show derived TPS from measured SimSpeed. This is not a direct engine tick counter. |
| `!profilerplus status` | Show current health, SimSpeed, derived TPS, process resources, pressure indicators, and world counts. |
| `!profilerplus snapshot` | Capture and display a fresh sample. |
| `!profilerplus report [minutes]` | Show a rolling health report. |
| `!profilerplus topgrids [count]` | Rank sampled grids by estimated pressure. |
| `!profilerplus grid <rank\|name\|entityId> [detailed]` | Inspect a grid. Quote names with spaces when needed. |
| `!profilerplus why <rank\|name\|entityId>` | Explain a grid-pressure score and its evidence. |
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
| `!profilerplus compare windows [beforeMinutes] [afterMinutes]` | Compare adjacent retained before/after windows and rank the largest changes. |
| `!profilerplus network` | Show opt-in aggregate replication state. It excludes endpoint/IP data. |
| `!profilerplus components` | Show opt-in loaded managed-assembly inventory for update correlation. |
| `!profilerplus fleet` | Show sanitized shared-directory fleet summaries when configured. |
| `!profilerplus overhead` | Show profiler collection overhead and Discord queue depth. |
| `!profilerplus export [minutes]` | Write rolling history to a local CSV export. Admin permission required; Discord gets a safe status only. |
| `!profilerplus data` | Show the local profiler data directory. Admin permission required; Discord gets a safe status only. |

## Administrator service controls

| Command | Purpose |
|---|---|
| `!profilerplusadmin help` | Show administrator controls. |
| `!profilerplusadmin status` | Show service status, configuration, and webhook state. |
| `!profilerplusadmin start` / `stop` | Start or pause sampling. |
| `!profilerplusadmin reload` | Validate and reload `TROA-ProfilerPlus.cfg`. |
| `!profilerplusadmin interval <seconds>` | Set the sampling interval within the supported range. |
| `!profilerplusadmin physics takeme <index>` | Best-effort teleport to an estimated physics cluster. |
| `!profilerplusadmin network clients [count]` | Sensitive local-only endpoint/IP list. Requires `EnableSensitiveClientNetworkDiagnostics=true`; never mirrored, logged, exported, bundled, or retained. |
| `!profilerplusadmin supportbundle [minutes]` | Create a sanitized local support ZIP. Review before sharing. |

## Administrator Discord controls

Configure `EnableDiscordWebhook` and a private webhook URL first. The universal mirror covers ordinary panel commands; these dedicated commands enqueue an on-demand, purpose-titled embed.

| Command | Purpose |
|---|---|
| `!profilerplusadmin webhook status` | Show safe webhook configuration/delivery status. |
| `!profilerplusadmin webhook mirror <on\|off>` | Enable or disable universal command-result mirroring. |
| `!profilerplusadmin webhook test` | Queue a test embed. |
| `!profilerplusadmin webhook health` / `top` / `world` / `overhead` / `digest` | Queue server-health, top-grid, world, overhead, or combined embeds. |
| `!profilerplusadmin webhook tick` | Queue the derived tick-rate/SimSpeed embed. |
| `!profilerplusadmin webhook why <rank\|name\|entityId>` | Queue a selected grid's pressure-score explanation. |
| `!profilerplusadmin webhook grid <rank\|name\|entityId>` | Queue a grid-analysis embed. |
| `!profilerplusadmin webhook player <name-or-id>` | Queue a player-associated workload embed. |
| `!profilerplusadmin webhook report [minutes]` / `incidents [count]` | Queue rolling-report or incident-archive embeds. |
| `!profilerplusadmin webhook entities` / `players [count]` | Queue entity diagnostics or player ranking embeds. |
| `!profilerplusadmin webhook timeline [minutes]` / `baseline` / `compare` | Queue timeline, learned-baseline, or comparison embeds. |
| `!profilerplusadmin webhook physics [count]` | Queue an estimated physics-cluster embed. |
| `!profilerplusadmin webhook network` | Queue aggregate network health; it excludes endpoint/IP data. |
| `!profilerplusadmin webhook fleet` | Queue sanitized fleet health. |
| `!profilerplusadmin webhook command <name>` | Queue a saved copy-ready command reference. |

## Administrator incident and correlation controls

| Command | Purpose |
|---|---|
| `!profilerplusadmin experiment start <name>` / `experiment end` | Capture and compare a controlled diagnostic experiment. |
| `!profilerplusadmin incident ack <id> [note]` | Acknowledge an incident. |
| `!profilerplusadmin incident note <id> <note>` | Append an administrator note. |
| `!profilerplusadmin incident escalate <id> [note]` | Mark an incident escalated and, when configured, deliver its alert. |
| `!profilerplusadmin marker <save\|cleanup\|update\|config> <detail>` | Record an operational marker and capture a pre-change snapshot when available. |
| `!profilerplusadmin markers [count]` | List recent operational markers. |

## Practical workflow

1. Save `BeforeUpdate` with `!profilerplus snapshot save BeforeUpdate`.
2. Save reference lines for your normal health, tick, grid, player, entity, and physics checks.
3. Apply one change and allow comparable samples to accumulate.
4. Run `!profilerplus compare windows 30 30`, inspect `topgrids`, `why`, `physics`, `network`, `components`, and `timeline`.
5. Use the universal mirror or a dedicated webhook command to place the relevant panel in Discord.
6. Create a support bundle only after reviewing what you intend to share.
