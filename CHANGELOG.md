# TROA Profiler+ Changelog

## v1.0.0-alpha.26 - stability recovery candidate (2026-10-28)

- Moves baseline and detached sampled-data analysis out of the blocking game-thread callback for regular timer captures.
- Caps low-speed detailed inspection at 10 grids per 60 seconds by default; inventory sampling continues and cached details may be older.
- Prunes removed-grid detail cache entries and excludes unloaded-world snapshots from incident evaluation.
- Coordinates with local Cleaner+ v1.12.0 recovery work; no automatic grid mutation added to Profiler+.
- Local ZIP under releases/; live testing remains with the owner.

Package and installation: [release notes](releases/v1.0.0-alpha.26.md). Live server validation is pending.

