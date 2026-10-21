# Ian · Nocturne Lab (`@ian-nocturne`)

> **"AI agents are great at writing 1,000 lines in 30 seconds—and terrible at telling you which 3 lines broke your build."**

Off-hours builder running **Nocturne Lab** late at night in Seoul.
I started building apps with Claude Code, Cursor, and Codex—until an agent cheerfully reported *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle, breaking my deploy, and painting the UI in unmistakable purple AI slop.

Now I build zero-dependency, deterministic guardrails for builders who ship with AI.

---

### What I Build

| Project | What It Does | Proof / Benchmarks |
|---|---|---|
| **[DeltaHarness](https://github.com/ian-nocturne/deltaharness)** | Polyglot AST instruction auditor for `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, and `SKILL.md`. Catches broken paths, renamed `npm/pnpm` scripts, and function signature drift before your agent hallucinates. | **12–230 ms** scan · Caught 5 real drifts in `vercel/ai` & 1 in `anthropics/claude-code` |
| **VibeRescue (`viberescue`)** | Secret-safe (`ZERO_SECRET_VALUES_INCLUDED`) triage & handoff bundler for stuck vibe-coded apps. Flags missing `.env` keys, exposed `"use client"` secrets, missing Supabase RLS, and competing lockfiles (`npm` vs `pnpm`), then generates `RESCUE_BRIEF.md` + `ONE_PROMPT_RESCUE.md`. | **< 10 ms** local triage · `rawEnvValuesIncluded: false` |
| **Anti-Slop Ship Gate (`anti-slop`)** | Deterministic UI & copy slop scanner (0–100 score, `Grade A+` to `F`). Strips purple radial blobs, `11.5px` fractional font sizes, emoji UI chrome, blocked mobile zoom, and locks reference-first `DESIGN.md` + `design-tokens.css` (`Linear` / `Toss` / `Stripe`). | Turned a 150-violation `Grade F` dashboard into **`Grade A+ (100/100)`** |

---

### 30-Second Quickstart

```bash
pip install git+https://github.com/ian-nocturne/deltaharness.git

# 1. Audit CLAUDE.md / AGENTS.md against your real codebase AST
deltaharness audit . --format markdown

# 2. Pack a secret-safe diagnostic brief & one-prompt repair plan
viberescue pack . --write

# 3. Scan AI UI slop and lock Reference-First design tokens
anti-slop scan . --format markdown
anti-slop lock . --archetype linear-precision
```

---

### House Rules at Nocturne Lab

1. **Never trust agent self-reports.** Only exit codes, AST symbol anchors, and SHA-256 digests count as done.
2. **Keep Mode by default.** If 1 file is broken, do not let the agent "refactor" 14 working files.
3. **Zero secret leaks.** Diagnostic bundles record `.env` key names and structural diffs only (`strict_keys_only`)—zero raw secret values ever leave your machine.
4. **Zero template slop.** 4-step integer typography, 3-step neutral mineral surfaces, 1px subtle hairlines, and zero emoji buttons.
