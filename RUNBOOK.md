# Runbook — Daily Check

Followed by the automated agent on every scheduled run. Read this file and
`watchlist.md` first, in full, before doing anything else.

## 1. Sync

```
git clone --depth 50 https://github.com/coffee-coded/pki-clm-market-watch <workdir>
```
(or pull if a workspace already exists). Work on `main` directly — no branches,
no PRs, this repo has a single reader/writer.

## 2. Load state

- Read `watchlist.md` for current scope.
- Read the last ~14 days of files under `findings/` (list the directory,
  sort by filename, read the most recent ones) so you know what's already
  been reported and don't duplicate it.

## 3. Research

For each primary vendor and the topic list in `watchlist.md`, search the web
(news search + the vendor's own newsroom/blog/press-release page when useful)
for anything published in roughly the last 24–48 hours (widen to 7 days if
this looks like the first run or a run was missed — check the most recent
`findings/` filename to tell). Prioritize:

- Primary vendors by name (Keyfactor, AppViewX, Venafi/CyberArk) — check these
  every run, even if you expect nothing.
- Funding, M&A, leadership changes, major product launches, breaking
  vulnerabilities/incidents, notable analyst reports (Gartner/Forrester) for
  the secondary vendors and topics.
- General industry/research signals (PQC migration deadlines, CA/Browser
  Forum ballots on certificate lifespans, notable cert-expiry outages) even
  when no specific vendor is named.

Skip anything already present in the recent `findings/` files (same story,
different outlet = still a duplicate; note the outlet only if it adds new
information).

## 4. Classify significance

Mark a finding **SIGNIFICANT** if it is any of:

- Funding round, acquisition, merger, or divestiture involving a watchlist
  company
- A primary vendor (Keyfactor, AppViewX, Venafi) shipping a major new
  product/capability, not a minor feature update
- A disclosed vulnerability, breach, or major outage involving a watchlist
  company or affecting CLM/PKI infrastructure broadly
- A C-level leadership change at a primary vendor
- A regulatory/standards change with a hard deadline (e.g. a CA/Browser Forum
  ballot passing, a NIST PQC deadline) that CLM buyers need to act on
- Major analyst report (Gartner Magic Quadrant, Forrester Wave) newly
  published for this category

Everything else that's real news but doesn't meet that bar is **routine** —
still log it, just don't flag it.

## 5. Log findings

If there is anything new (significant or routine), append to
`findings/<YYYY-MM-DD>.md` (create it if today's file doesn't exist yet).
Each entry:

```
### [SIGNIFICANT] <headline>
- **Date:** <publication date>
- **Source:** <outlet/publisher>, <url>
- **Summary:** 1-3 sentences, what happened and why it matters to Keyfactor/
  AppViewX/Venafi or the broader CLM/PKI market.
```

(Omit the `[SIGNIFICANT]` tag for routine entries.)

If there is nothing new today, do **not** create an empty file — just do
nothing for step 5.

## 6. Commit and push

Commit with a message like `findings: 2026-09-16` (only if you created/changed
a findings file). Push to `main`.

## 7. Weekly digest (Mondays only)

Check today's date. If today is Monday (or this run is explicitly the weekly
digest run), also:

- Read every `findings/` file from the past 7 days.
- Write `digests/<ISO-year>-W<ISO-week>.md`: a short digest grouped by
  vendor/topic, significant items called out at the top, routine items
  summarized in a few bullets below. Commit and push it too.

## 8. Final report (this is what becomes your notification)

End your turn with a plain-text summary, since it is what gets surfaced to
the user as a push notification / email:

- If nothing new: one line, e.g. "No new PKI/CLM news today — nothing to
  report." (This should read as clearly *not* noteworthy.)
- If only routine items: a short "quiet day" summary, 2-4 bullets.
- If anything is SIGNIFICANT: lead with `SIGNIFICANT:` followed by the
  headline(s) and why it matters, then the rest.
- On a Monday digest run: lead with the digest headline items, then note the
  full digest is committed to `digests/`.

Keep this final message itself concise (a phone notification, not an essay) —
the full detail belongs in the committed files, not the message.
