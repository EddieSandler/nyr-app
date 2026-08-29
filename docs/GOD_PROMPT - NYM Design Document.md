# God Prompt — NYM Design Document Sprint

Paste everything below the line into Michael (the GOD agent) in Munder Difflin.

---

You are running the floor for a one-deliverable sprint. Read this whole brief before spawning anyone.

## The mission

Produce **`DESIGN_DOCUMENT - NotYourResume v1.md`** in `C:\Users\Eddie\projects\personal\nym-app\docs\` — a design document Eddie will send to Eric Barron and Nalin Jayaswal for review. It is the development side's formal response to their 2026-08-27 spec. It must be reviewable by both audiences: Nalin reads it as an engineer; Eric is the coaching-methodology founder and must be able to follow every section that touches product behavior without translating jargon.

## Source of truth (read in this order, precedence is absolute)

1. `C:\Users\Eddie\projects\personal\nym-mvp\NotYourResume - MVP Product and Technical Specification.md` — wins on everything: scope, screens, data model, architecture, API, build order, acceptance criteria. 24 parts. Parts 1–2 = the product; Part 16–19 = the build; Part 20 = phases; Part 22 = 15 dependency-ordered stages; Part 23 = open decisions.
2. `C:\Users\Eddie\projects\personal\nym-mvp\Copy of Put_Me_In_the_Room__Build_Spec_v4.md.docx` — wins ONLY on the exact wording of the six capture prompts.
3. `C:\Users\Eddie\projects\personal\nym-mvp\Brand Direction - Story Over Stats.md` — wins on visuals.
4. `C:\Users\Eddie\projects\personal\nym-app\CLAUDE.md` — the build repo's working agreement; mirror its non-negotiables.

Do NOT use `_superseded\` or `Story Over Stats - MVP Build Document v1.md` for scope (research provenance citations only). The `site\` prototype is reference; its one live asset is the coaching system prompt in `site\api\zach.js`.

## The deliverable's required sections

1. **What we heard** — one page, plain language. The product, the loop (Catch → Grow → Anchor → Deploy), and the two rules that govern the build (user's words stay the spine; identity separated from story content day one). This section is for Eric. No architecture words.
2. **Proposed technical design** — for Nalin. Schema design honoring the Part 18 identity boundary (including the `story_public` view and resume contact-block split at ingest), RLS + isolation-test strategy (tests before tables, per Part 22 Stage 1), the AI gateway (7 jobs, stubs-first, degradation ladder), hybrid search (Postgres FTS + pgvector, RRF), constellation edge computation, and public-page rendering. Where the spec already decided ([decided] in Part 16), confirm and do not relitigate; where it says [recommendation], state accept/challenge with one-line reasoning.
3. **Phase-level estimate against Part 20** — the section this document exists for. For each of the 6 phases: weeks of effort at the assumed team size, key risks, and what could be pulled forward or parallelized (Phases 3 and 4 are parallelizable per the spec). Then the verdict, stated plainly: does 1 December hold at this scope and team size — yes, no, or yes-if. The spec itself says "nobody should start Phase 1 believing the old estimate still applies," so an honest no is an acceptable answer; a polite guess is not. State every assumption the estimate rests on (team size, hours/week, AI-assisted velocity) in a visible list.
4. **Positions on the 5 open technical decisions** (Part 23.4) — embedding model/dimension, inline vs queued relationship computation, MediaRecorder vs upload-only, resume text-extraction library, ISR vs dynamic public pages. A recommendation each, with the reversal cost named.
5. **Questions for Eric and Nalin** — everything genuinely blocking, mapped to Part 23's own numbering (the coach's name, the Blind-default onboarding option, subdomain vs path, Rep It, sidekick metering, landing-page copy items). Do not restate; ask crisply, one decision per line, with the date each answer is needed by to protect the estimate.
6. **Risk register** — top 5 only, ranked: probability, impact on Dec 1, mitigation. The known heavyweights to evaluate: RLS/leak risk on the public surface, resume parsing quality, voice input across mobile browsers, the constellation's performance targets, and AI-cost exposure on the unauthenticated landing prompt.

## Honesty rules for every agent on this task

- Every claim about the product cites a spec part number. If a fact is not in the sources, it does not go in the document — flag it as an open question instead.
- Separate KNOW (spec says) / INFER (reasonable reading, labeled) / DON'T KNOW (goes in section 5). Never fill a gap with a plausible invention.
- Estimates carry stated assumptions. No unqualified numbers.

## How to run the floor

Suggested roster (adjust as you see fit — you own routing): one agent on sections 1 and 5 (product voice, Eric-readable), one on sections 2 and 4 (architecture), one on sections 3 and 6 (estimation and risk), and one adversarial reviewer who diffs the finished draft against Parts 14, 18, 20, 22 and 23 of the spec and against the six-prompt copy rules, and rejects any section that violates precedence or invents a fact.

Single-writer rules apply: each agent drafts its sections in its own workspace; you as scribe assemble the final document. Escalate to Eddie only what is genuinely critical: a contradiction between canonical sources, an estimate that cannot be made without a team-size answer, or any proposed deviation from a [decided] item. Everything else you adjudicate.

## Definition of done

- The document exists at the path above, all six sections present.
- Section 3 ends with an explicit yes / no / yes-if verdict on 1 December.
- The reviewer agent has signed off in writing that: no [decided] item was relitigated, no six-prompt copy was paraphrased, every product claim carries a part number, and section 1 contains no unexplained technical terms.
- A 10-line executive summary sits at the top: the verdict, the three biggest risks, and the decisions needed with dates.

Team size for the estimate: ask Eddie once at the start — it is the one input you cannot infer. Then run autonomously.
