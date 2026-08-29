# CLAUDE.md — NotYourResume build (nym-app)

Production build of **NotYourResume**: a fixed six-question coach turns raw professional
moments into indexed, retrievable stories, connected into a visual constellation and to a
public interactive resume. Founding cohort: 50–100 members at $100, cart opens 2026-09-02,
**platform live 2026-12-01**.

## Source-of-truth documents (precedence order — read before building)

All in `../nym-mvp/`:

1. `NotYourResume - MVP Product and Technical Specification.md` — wins on scope, features,
   screens, data model, architecture, API surface, build order, acceptance criteria.
2. `Copy of Put_Me_In_the_Room__Build_Spec_v4.md.docx` — wins ONLY on the exact wording of
   the six capture prompts. Diff against it. Never paraphrase it.
3. `Brand Direction - Story Over Stats.md` — wins on visuals: ink/paper/flare palette,
   nine color families × three intensities.

Never build from `../nym-mvp/_superseded/` or `Story Over Stats - MVP Build Document v1.md`
(superseded on scope). `../nym-mvp/site/` is a reference prototype, not a codebase to
extend — carry over the coaching system prompt from `site/api/zach.js`, rewrite the plumbing.

**Naming:** the product is NotYourResume. "Story Over Stats" is the thesis line and legal
entity. The coach's user-facing name is OPEN (decision #1: "Story Coach" vs "Zach" vs
nothing) — keep it in config, never hardcoded in strings.

## Non-negotiables (from the spec; violating any of these is a defect)

1. **The six-question spine is fixed.** Order never changes; AI never chooses, reorders,
   skips, or rewrites the questions. AI does exactly four in-flow jobs: probes (prompts
   1/4/5 only, at most once per field, appending never replacing), trait chips (user
   confirms, never auto-applied), two-version drafting (60-sec + 5-min, editable), and
   metadata extraction.
2. **The user's words stay the spine.** AI suggests; it never replaces user text with
   generated prose. The six spine fields are discrete columns, never JSON, never
   overwritten by generated versions.
3. **Identity is separated from story content at the schema level, day one.**
   Identifying columns live on `profiles` and nowhere else. No denormalized
   `author_name`/`employer` anywhere on the story side. Build the `story_public` view
   (identity join omitted) even before anything queries it. The resume contact block is
   split from the body at ingest.
4. **RLS on every user-owned table, scoped to `auth.uid()`, with isolation tests written
   BEFORE the tables** (Part 22, Stage 1). Test through API routes AND direct PostgREST
   with a user token. "A leak is not a bug, it is the end of the company."
5. **Everything degrades, nothing hard-breaks.** One AI gateway module; every model call
   has a stub that works with no API key; capture must complete end-to-end with scripted
   fallbacks. A spark is never lost, offline included.
6. **Never ask the user for a metric.** No output ever requests numbers. The coach never
   refers to itself as an AI.

## Stack (all [decided] in Part 16)

Next.js App Router + React + TypeScript (strict) as an installable PWA · Supabase
(Postgres, Auth, RLS, Storage) · pgvector for hybrid search + constellation edges ·
one AI gateway over Claude, server-side only, 7 jobs (probe, traits, drafting, metadata,
embedding, resume structuring, prompt personalization) · Vercel · Stripe Checkout ·
first-party `events` table. Build for 100 concurrent users; do not engineer for scale.

## Build order

Part 20 (6 phases) is construction order, not priority tiers — everything ships before
launch. Part 22 (15 stages, with dependencies) is the granularity to work at.
**Start: Stage 1 — schema + RLS with the isolation suite written first.**

## Conventions for this repo

- Prompt templates are versioned files in the repo, never inline strings; every stored AI
  output records its template version.
- Every AI-written DB field carries `source` ('ai'|'user') and `confirmed_at`; once a user
  edits a field, regeneration must not clobber it.
- Log tokens per AI call type from day one.
- Structured outputs validated against a schema, one retry, then graceful fallback.
- Every authenticated route resolves the user from the session, never a request param.
- Record architectural decisions in `docs/DECISIONS.md`; track spec Part 23 items in
  `docs/OPEN_QUESTIONS.md`. Update both in the same session the decision happens.

## Working with Eddie (ADHD accommodations + skill maintenance)

- Lead with the next action. Number multi-step work. One question at a time.
- Restate where we are in the build every session ("Stage N of 15, X done").
- **Explain every line of code written.** Full docstrings and comments — Eddie is a
  Python/full-stack dev keeping his skills current, not just shipping.
- Do not guess. If a fact is missing, ask or flag it as missing. If the spec and this
  file ever disagree, the spec wins — and tell Eddie the discrepancy.
