# CONTEXT.md — TROA Profiler+ (public release repo)

Durable context for this repository.

## Purpose

Public distribution point for **TROA Profiler+**, a headless performance-intelligence plugin for Torch
(Space Engineers dedicated servers). Server owners download the built ZIP here and read the docs here.

## Repo relationship

- **This repo (`troainc/TROA-Profiler-Plus`)** — docs + built ZIP only. Public.
- **`troainc/TROA-Profiler-Plus-Closed`** — private C# source. All code changes happen there; builds are
  published here.

## Current shipped release

- `TROA-ProfilerPlus-v1.0.0-alpha.8.zip` (see `README.md` for the SHA-256 and feature list).

## In flight: v1.0.0-alpha.9 (pending build)

The source PR `troainc/TROA-Profiler-Plus-Closed#1` adds:

- **Estimated physics clusters** — `!profilerplus physics <count>` / `physics inspect <index>` and admin
  `physics takeme <index>`, mirroring the classic cluster profiler's UX with estimated (not measured)
  per-cluster ms/frame. Real measured timing is reserved behind an opt-in, fail-closed game-loop probe.
- **Universal command webhook mirror** — `EnableCommandWebhookMirror` / `webhook mirror on|off` posts every
  command result to Discord as an embed, plus dedicated embeds for all remaining report types.

These are **not yet shipped here** — this repo will get an alpha.9 ZIP + doc update once the DLL is built
and verified on a Windows/Torch box. Until then the current release stays alpha.8.

## What lives in the plugin (for reference)

Interval sampling of SimSpeed, process/host resources, and world state; explainable health score and
estimated grid/physics/entity pressure; incidents, baselines, timelines, operational markers; polished
in-game panels and Discord embeds; CSV/JSON/Prometheus outputs and a Grafana dashboard. It never repairs
or deletes world data. Webhook URLs are treated as secrets and never displayed or logged.
