# HANDOFF — MERCY session pickup

Fresh-session orientation for MERCY, a PySide6 + SQLite desktop app: part costing (ANIKA) + HR (BECKY) + per-employee production tracking + the production-scheduling subsystem. Read this file cold at the start of every session; dive into the other docs only where they're called out.

## 🧭 Cursor

**The ONE home for live status.** Every other doc (and the agent memory) points here instead of restating it; when state changes, this table is the single place that updates. **Hard cap: ~15 lines.** If something doesn't fit, it belongs in ROADMAP (forward) or WORKLOG (backward), with at most a pointer here — a Cursor that only grows is how the last doc died.

| | |
|---|---|
| **NEXT** | **FROZEN** (2026-09, transition decision 8) — desktop is **bugfix-only**; the web rebuild drives from [TKG-Software/mercy-web](https://github.com/TKG-Software/mercy-web) (its `TRANSITION_PLAN.md` is the plan of record). Step 86+ shelved in [ROADMAP](ROADMAP.md). |
| **Last landed** | **Bugfixes for the release of 2026-10-30** (2026-10-29 and 2026-10-30, [WORKLOG](WORKLOG.md)): Daily Entry's edit window and Batch Entry refuse a production date after today, and a production report's Total that does not fit at a page's foot goes on the next page; Matthew's manual check on Windows passed for both (2026-10-29, `check_release_2026-10-30.db`). On 2026-10-30, at his word, the sales and inventory reports' Totals the same way, and then the rest, held for in the same release: the employee reports' Totals, and a mixture report that does not fit on its page says so; his manual check passed that day (`check_report_rows.db`). **Released as `v2.0rc8`** (2026-10-30, [GitHub pre-release](https://github.com/TKG-Software/TKG-MERCY/releases/tag/v2.0rc8), `mercy-2.0rc8.exe`) with all four, and emailed to Rich. Before it: **bugfix from the web port's production audit** (2026-10-27): the production edit window keeps a part, mixture or employee no longer listed instead of saving the first in the list. **Released as `v2.0rc7`** (2026-10-27, [GitHub pre-release](https://github.com/TKG-Software/TKG-MERCY/releases/tag/v2.0rc7), `mercy-2.0rc7.exe`) after Matthew's manual check on Windows passed (`check_unlisted_target.db`). Before it: **bugfixes from the web port's employee audit** (2026-10-21) — **released as `v2.0rc6`** (2026-10-21, [GitHub pre-release](https://github.com/TKG-Software/TKG-MERCY/releases/tag/v2.0rc6), `mercy-2.0rc6.exe`) after Matthew's manual check on Windows passed for all five; T1.7 tab-usage telemetry (2026-09-04, the freeze's one sanctioned add), the freeze itself, 85 (single-source facts machine-enforced via `docs_single_source` + the one-place check registry), 84-post-triage (the "wiped DB" divide-by-zero), 83, 84 (the doc split; `MERGE_PLAN.md` retired to [plan_archive/merge_plan.md](plan_archive/merge_plan.md)). |
| **Smoke baseline** | **89 PASS** — `./Scripts/python.exe -m smoke`. Quoted here and nowhere else, and `docs_single_source` now fails if that stops being true. |
| **Branch / tree** | `main`, working tree clean at last close |
| **Carried watch-items** | Step 80's real-floor gate **unmet** (dies-stop-hopping needs a deployed schedule + real `Press.currentPart` data); `dieChangeHours` deliberately **0.0** — do **not** invent a value; scheduler-vs-real-orders validation open. All tracked in [ROADMAP](ROADMAP.md) § Blocked on real data. Small open bugs, each needing Matthew's agreement: [ROADMAP](ROADMAP.md) § Open bugs. |

**Two rules for reading step status (both load-bearing):**

1. **"Landed" means the code shipped and its automated checks pass — NOT that the step's real-data / floor gate cleared.** Live example: Step 80. When a step's *confidence* matters, read its WORKLOG entry, not the tick.
2. **Steps land out of numeric order** (Step 82 shipped before 81). Derive release scope from the actual tag range, never from step numbers.

## The doc system

Adopted at Step 84 (2026-07-17), adapting the shape of Matthew's `asciibattler` project to MERCY's flow: no pre-specced rounds here — work arrives as team feedback, from one-line changes to multi-step blocks, so the files are **permanent** and rotate by size, not per-round.

- **[HANDOFF.md](HANDOFF.md)** (this file) — orientation + the 🧭 Cursor. Rarely changes outside the Cursor.
- **[ROADMAP.md](ROADMAP.md)** — forward-looking only; **shrinks** as work lands. Holds each planned step's scoping essay plus the blocked / waiting backlogs.
- **[WORKLOG.md](WORKLOG.md)** — backward-looking as-built record; **grows**, newest first. Entry length scales with the step: a one-line change gets one line, a subsystem gets paragraphs.
- **[CONVENTIONS.md](CONVENTIONS.md)** — durable dev conventions + gotchas. The graduation target: when a WORKLOG narrative carries a lesson future work must obey, the *lesson* moves here and the narrative stays history.
- **[plan_archive/](plan_archive/)** — closed history, including the retired **[merge_plan.md](plan_archive/merge_plan.md)** (the original merge design + Steps 1–83 scoping/as-built essays). `MERGE_PLAN §12.x / §13.x` references in code comments resolve there.

**Lifecycle of a step:** team request → scoping essay in ROADMAP → build (one step = one commit) → in the landing commit: as-built entry in WORKLOG, ROADMAP entry deleted, Cursor updated → durable lessons graduate to CONVENTIONS.

**Rotation rule** (what keeps WORKLOG from becoming the next behemoth): when WORKLOG passes ~300 lines, rotate everything but the newest ~10 steps to a dated `plan_archive/worklog_YYYY-MM.md`, first condensing any still-load-bearing facts into CONVENTIONS or a Cursor carried-item. A completed multi-step block may rotate as a unit sooner.

**Single-source rule:** a live fact (smoke count, next step, current real-DB filename) lives in exactly **one** doc; everything else points at it. Every doc rot found in the 2026-07-16 cold-read audit was one fact stated twice with one copy updated.

## History in one breath

The original ANIKA + BECKY three-way merge completed at Step 13 (2026-04); everything since is post-release work driven by team feedback. The blocks: UI / report polish (14–27) · refactor + code quality (28–36) · UI-test + crash-fuzz hardening (37–41, 55–62) · the **Production Scheduling subsystem** (42–54 — spec: [prod-sched-spec.md](plan_archive/prod-sched-spec.md), algorithm: [prod-sched-algorithm.md](plan_archive/prod-sched-algorithm.md)) · press-preference grids + presser staffing + report UX (63–68) · trucks entry + sort modes + schedule-tab polish (69–75) · die constraint + orders PDF report (76–78) · die tracking + hysteresis + whole-piece quantities (79–82) · the doc split (84). Full narratives: [plan_archive/merge_plan.md](plan_archive/merge_plan.md) §13 + [plan_archive/implementation_notes.md](plan_archive/implementation_notes.md).

## Baseline workflow

Run `./Scripts/python.exe -m smoke` at the start and end of any invasive change — the offscreen check battery takes a few seconds and is the regression net (details, real-DB drill rules, and headless-Qt gotchas: [CONVENTIONS.md](CONVENTIONS.md)). **Manual GUI testing on Matthew's Windows machine is the acceptance bar for anything user-facing** — smoke can't reach combo visibility / rebuild / selection / in-cell commit paths, so pause for his sweep before committing Qt-UI-heavy work.
