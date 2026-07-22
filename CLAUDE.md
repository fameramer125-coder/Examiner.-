# CLAUDE.md — THE EXAMINER (Universal Edition)

Persistent project memory. Read this first every session so context does not have to be re-explained.

## What this is
A single-file, client-side web app: **`index.html`** (~60 KB). An adversarial manuscript critic — "THE EXAMINER" — that builds a bespoke critique profile of a submitted manuscript, then spars with the author. Pure HTML/CSS/JS. **No build step, no backend, no dependencies.** It runs from `file://` or any static host.

## Architecture (everything lives in `index.html`)
- One file: inline `<style>` + `<body>` + `<script>`. No bundler, no npm, no framework.
- **Model API:** Google Gemini `gemini-2.0-flash` via `https://generativelanguage.googleapis.com`.
- **API key:** user-supplied at runtime, stored only in browser `localStorage` under `examiner_api_key`. It is **never** committed and never leaves the browser except in direct HTTPS calls to Google.
- **Domains** (`DOMAINS` object): `research`, `academic_book`, `children`, `adult_fiction`, `islamic`, `arabic_ling`. Each has its own attack `modes` and a `systemCore` prompt.
- **Controls:** severity 1–5 (`SEV_LABELS`/`SEV_PROMPTS`), per-domain attack modes, rigour-score parsing from the model's `RIGOUR SCORE: n/100` line.
- **Flow:** onboarding (paste/upload manuscript) → analysis (builds JSON profile) → chat (dissection with `[FATAL FLAW]`/`[MAJOR WEAKNESS]`/… tags rendered as pills).

## Critical facts / gotchas
- **History lesson:** commit `669ed4f` once destroyed the app by overwriting the full source with only its rendered plain text (982 bytes). It was restored in PR #1. **When editing, never paste the rendered page text over the source — keep the complete HTML.**
- The file is **UTF-8** with Arabic text and emoji (domain icons, the RTL Arabic key panel). Preserve encoding on every edit.
- **No secrets in the repo.** Do not embed any API key.

## Conventions
- Keep it a **single self-contained file**. That portability is a feature, not an accident.
- UI copy uses **British spelling** ("Analyse", "Rigour", "-ise").
- Fonts via Google Fonts import: Libre Baskerville (serif), DM Mono (mono), Outfit (sans).

## Deploy
- Any static host works with zero config: Netlify drop, GitHub Pages, or open `index.html` directly. No build command, publish directory = repo root.

## Git / PR workflow
- Develop on a feature branch; open the PR as a **draft**.
- Commit identity: `Claude <noreply@anthropic.com>`.
- After the branch's PR merges, restart the branch from the latest `main` for follow-up work (do not stack on merged history).
