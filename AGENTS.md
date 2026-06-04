# AGENTS.md

## Project

MarketLab is a fake-money, binary Yes/No prediction-market-inspired app for a 2-hour Cursor workshop. Keep changes small, reviewable, workshop-friendly, and easy for participants to follow.

## Stack

- Next.js App Router with React Server Components by default.
- Server Actions for mutations.
- Supabase Auth and Database via `@supabase/ssr` and `@supabase/supabase-js`.
- Bun, TypeScript, Tailwind CSS, Biome, Vitest, and Task.

Do not add dependencies or replace the stack unless the user explicitly asks. Before editing Next.js APIs or routing behavior, read the relevant local guide in `node_modules/next/dist/docs/`; this repo may use newer conventions than your training data.

## Project Map

- `src/app/`: App Router routes, layouts, and global styles.
- `src/components/`: Reusable UI and MarketLab components.
- `src/lib/`: Shared utilities and Supabase clients.
- `supabase/`: Supabase configuration, migrations, and seed data.
- `Taskfile.yml`: Source of truth for project commands.

## Commands

Run commands through `task` when a task exists.

- Setup: `task setup`
- Dev server: `task dev`
- Format/lint check: `task check`
- Typecheck: `task typecheck`
- Unit tests once: `task test:run`
- Full verification: `task verify`
- List tasks: `task --list`

If `task` is unavailable, activate mise in the current shell first, then retry the `task` command. Do not use `mise exec -- task`.

## Testing Expectations

- Add focused Vitest tests as features are developed, especially for balance math, share purchases, and authorization.
- Keep tests narrow, readable, and tied to the workshop feature slice.
- Run the smallest relevant check while iterating; finish with `task verify` when practical.

## Cursor Workflow

- Build one feature slice at a time.
- Add focused tests for the slice.
- Run the smallest relevant check.
- Explain the important diff and any remaining risk.

## Implementation Rules

- Prefer Server Components. Add `"use client"` only for browser-only state, effects, or event handlers.
- Keep UI components focused and workshop-readable; avoid broad refactors and clever abstractions.
- Use existing components and utilities before creating new ones.
- Use binary Yes/No markets only.
- Keep all market examples fictional and non-political.
- Treat balances as fake cents; the simplified workshop rule is `1 fake cent spent = 1 share cent`.
- Balance-changing writes must happen server-side.
- Out of scope: real payments, real betting behavior, financial claims, trading advice, political markets, dynamic pricing, automated market makers, order books, limit orders, selling shares, liquidity pools, comments, notifications, analytics, production monitoring, and transactional email features.

## Supabase Rules

- Supabase migrations are the schema source of truth.
- Every Server Action must verify authentication and authorization before mutation.
- Prefer RLS and Supabase RPC functions for balance-changing operations.
- Public market data may be readable; profiles, positions, and ledger entries must stay owner-scoped.
- After adding migrations or seed data, run `task db:push` and `task db:types` once the repo is linked to the hosted Supabase project.

## Validation

- For code changes, finish with `task verify` when practical.
- For hook changes, run `task hooks:validate` and `task hooks:run`.
- If a required check cannot run, report the reason and the remaining risk.
