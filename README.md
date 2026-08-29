# nym-app — NotYourResume

Production build of NotYourResume. The MVP spec, prompt copy, and brand direction live in
`../nym-mvp/` — see `CLAUDE.md` in this repo for the precedence rules and non-negotiables.

- Spec: `../nym-mvp/NotYourResume - MVP Product and Technical Specification.md`
- Timeline: cart opens 2026-09-02 · platform live 2026-12-01
- Stack: Next.js PWA · Supabase (Postgres/Auth/RLS/Storage) · pgvector · Claude (one gateway) · Vercel · Stripe

## Status

Scaffold only. App code not yet generated. First build step per spec Part 22:
Stage 1 — schema + RLS with isolation tests written first.

## Repo docs

- `CLAUDE.md` — working agreement + guardrails for AI-assisted development
- `docs/DECISIONS.md` — architecture decision log
- `docs/OPEN_QUESTIONS.md` — tracker for spec Part 23 open decisions
