# Ian · Nocturne Lab (`@ian-nocturne`)

> **"In the hospital, nobody accepts 'All fixed!' without checking vitals and a pre-op checklist. I apply that same surgical discipline to AI coding agents at 2 AM."**

Young MD & post-call builder running **Nocturne Lab** late at night in Seoul.
I started building apps after night shifts with Claude Code, Cursor, and Codex—until an agent cheerfully reported *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle, breaking my deploy, and painting the UI in unmistakable purple AI slop.

Now I build zero-dependency, surgical verification tools (**First, do no harm: touch only the broken lines, never the healthy code**) for builders who ship with AI.

---

### What I Build

| Project | What It Does | Vitals / Proof |
|---|---|---|
| **[DeltaHarness](https://github.com/ian-nocturne/deltaharness)** | Pre-op timeout checklist & polyglot AST auditor for `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, and `SKILL.md`. Catches broken paths, renamed `npm/pnpm` scripts, and function signature drift before your agent hallucinates. | **12–230 ms** scan · Caught 5 real drifts in `vercel/ai` & 1 in `anthropics/claude-code` |
| **VibeRescue (`viberescue`)** | Secret-safe (`ZERO_SECRET_VALUES_INCLUDED`) triage & handoff bundler for stuck vibe-coded apps. Flags missing `.env` keys, exposed `"use client"` secrets, missing Supabase RLS, and competing lockfiles (`npm` vs `pnpm`), then generates `RESCUE_BRIEF.md` + `ONE_PROMPT_RESCUE.md`. | **< 10 ms** local triage · `rawEnvValuesIncluded: false` |
| **Anti-Slop Ship Gate (`anti-slop`)** | Deterministic UI & copy slop scanner (0–100 score, `Grade A+` to `F`). Excises purple radial blobs, `11.5px` fractional font sizes, emoji UI chrome, blocked mobile zoom, and locks reference-first `DESIGN.md` + `design-tokens.css` (`Linear` / `Toss` / `Stripe`). | Turned a 150-violation `Grade F` dashboard into **`Grade A+ (100/100)`** |

---

### 30-Second Triage Quickstart

```bash
pip install git+https://github.com/ian-nocturne/deltaharness.git

# 1. Pre-op timeout audit: check CLAUDE.md / AGENTS.md against your real codebase AST
deltaharness audit . --format markdown

# 2. Emergency triage: pack a secret-safe diagnostic brief & one-prompt surgical rescue
viberescue pack . --write

# 3. Excise AI UI slop and lock Reference-First design tokens
anti-slop scan . --format markdown
anti-slop lock . --archetype linear-precision
```

---

### Operating Room Rules at Nocturne Lab

1. **Check vitals, never self-reports.** Only exit codes, AST symbol anchors, and SHA-256 digests count as done.
2. **First, do no harm (`Keep Mode`).** If 1 file is broken, do not let the agent "refactor" 14 healthy files.
3. **Confidentiality by default.** Diagnostic bundles record `.env` key names and structural diffs only (`strict_keys_only`)—zero raw secret values ever leave your machine.
4. **Zero template slop.** 4-step integer typography, 3-step neutral mineral surfaces, 1px subtle hairlines, and zero emoji buttons.
