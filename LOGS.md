# LOGS.md — TROA Profiler+ (public release repo)

Reverse-chronological log of notable documentation/publish sessions.

## 2026-09-23 — document alpha.9 features (build pending)

Branch: `troainc/brave-ptolemy-fl4i7o`

- Added repo context files: `AGENTS.md`, `CONTEXT.md`, `LOGS.md`.
- Recorded the in-flight **v1.0.0-alpha.9** work (source PR
  `troainc/TROA-Profiler-Plus-Closed#1`): estimated physics clusters
  (`!profilerplus physics` / `physics inspect` / admin `physics takeme`) and the universal command webhook
  mirror plus dedicated embeds. Noted in `CHANGELOG.md` as pending build.
- **No ZIP published this session.** The alpha.9 DLL must be built and verified on a Windows/Torch box; the
  shipped release stays `TROA-ProfilerPlus-v1.0.0-alpha.8.zip` until then. The build could not run in the
  Linux cloud sandbox (no Torch/SE references, no offline dotnet).

**Next publish:** build alpha.9 from the closed repo, commit `TROA-ProfilerPlus-v1.0.0-alpha.9.zip`, update
`README.md` (version/package/SHA-256) and `CHANGELOG.md`, and remove the superseded alpha.8 ZIP.
