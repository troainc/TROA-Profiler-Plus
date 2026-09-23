# AGENTS.md — TROA Profiler+ (public release repo)

Guidance for AI agents and contributors working in this repository.

## What this repo is

This is the **public release / distribution** repo for the TROA Profiler+ Torch plugin. It contains
**only** documentation and built artifacts — **no source code**:

- `README.md`, `CHANGELOG.md`, `LICENSE.md`
- `.github/ISSUE_TEMPLATE/`
- `TROA-ProfilerPlus.cfg.example`
- `TROA-ProfilerPlus-vX.Y.Z-*.zip` — the built Torch plugin (compiled DLL + manifest + dashboard + cfg)

The real C# source lives in the private repo **`troainc/TROA-Profiler-Plus-Closed`**. Make code changes
there, build the ZIP on a Windows/Torch box, then publish the ZIP + updated docs here.

## Rules

- **Do not put source here.** No `.cs`, `.csproj`, or `Tests/` — those belong in the closed repo.
- **Do not hand-edit the ZIP** or claim a version is released before its ZIP is actually built and its
  SHA-256 computed. Keep `README.md` "Current Release" in sync with the ZIP that is actually committed.
- Keep `TROA-ProfilerPlus.cfg.example` here in sync with the source repo's copy.
- Update `CHANGELOG.md` when publishing a new build; record notable doc/publish sessions in `LOGS.md`.

## Release checklist (run on a Windows/Torch build box)

1. In the closed repo: `dotnet build -c Release -p:TorchDirectory=... -p:SpaceEngineersBin64=...` and
   `dotnet run --project Tests`.
2. Take the produced `TROA-ProfilerPlus-vX.Y.Z.zip`, compute its SHA-256.
3. In this repo: commit the ZIP, update `README.md` (version, package name, SHA-256), `CHANGELOG.md`, and
   `TROA-ProfilerPlus.cfg.example` if config changed. Remove superseded ZIPs.
