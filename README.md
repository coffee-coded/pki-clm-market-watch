# PKI / CLM Market Watch

Automated market-intelligence tracker for the PKI (Public Key Infrastructure) and
CLM (Certificate Lifecycle Management) space — vendor moves, funding, product
launches, research, and industry news, with a focus on **Keyfactor**,
**AppViewX**, and **Venafi**, plus the broader competitive set.

This repo is the persistent memory for a recurring automated research job. It
is not meant to be edited by hand day-to-day — a scheduled agent run appends to
it. You (or anyone with repo access) read it to catch up on the space.

## How it works

1. A daily scheduled job scans for new PKI/CLM news, research, and vendor
   announcements (see `watchlist.md` for what's tracked).
2. New items are deduplicated against everything already logged, then appended
   to `findings/<YYYY-MM-DD>.md`.
3. If a finding looks **significant** (funding round, acquisition, major
   product launch, breaking vulnerability/incident, executive change, big
   competitive move), the job sends an immediate push notification + email.
   Otherwise it's just logged for the weekly roll-up.
4. Every Monday, a second job reads the past week's `findings/` files and
   writes a digest to `digests/<YYYY>-W<WW>.md`, and sends that digest as a
   notification.

## Layout

- `watchlist.md` — companies and keywords the job searches for. Edit this to
  change scope.
- `RUNBOOK.md` — the exact instructions the automated agent follows each run
  (search strategy, significance criteria, logging format). Edit this to
  change *how* the job behaves.
- `findings/` — one file per day that had new findings, oldest to newest
  entries within each file.
- `digests/` — one weekly summary file per ISO week.

## Adjusting the automation

The recurring runs are Claude Code Routines (scheduled triggers), not GitHub
Actions — there's no workflow file in this repo to edit. To change cadence,
scope, or notification settings, ask Claude (in a Claude Code session) to
update the routine, or edit `watchlist.md` / `RUNBOOK.md` for scope and
behavior changes, which the next scheduled run will pick up automatically.
