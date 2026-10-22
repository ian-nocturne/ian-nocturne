# Ian · Nocturne Lab (`@ian-nocturne`)

> **"AI agents are great at writing 1,000 lines in 30 seconds—and terrible at telling you which 3 lines broke your build."**

Off-hours builder running **Nocturne Lab** late at night in Seoul.
I started building apps with Claude Code, Cursor, and Codex—until an agent cheerfully reported *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle, breaking my deploy, and painting the UI in unmistakable purple AI slop.

Now I build zero-dependency, deterministic guardrails for builders who ship with AI.

---

### What I Build

| Project | What It Does | Proof / Benchmarks |
|---|---|---|
| **[DeltaHarness](https://github.com/ian-nocturne/deltaharness)** | Static instruction auditor (`Python AST` + `TS/JS/Go/Rust` lexical symbol scanner) for `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, and `SKILL.md`. Catches broken paths, renamed `npm/pnpm` scripts, and function signature drift before your agent hallucinates. | Audited **784 docs / 2,745 refs** across `vercel/ai`, `openai-agents-python`, `AutoGPT`, & `claude-code` · **10 real broken paths caught, 0 false positives** |
| **VibeRescue (`viberescue`)** | Secret-redacted (`SECRET_REDACTED_BUNDLE`) triage & handoff bundler for stuck vibe-coded apps. Flags missing `.env` keys, exposed `"use client"` secrets, missing Supabase RLS, and competing lockfiles (`npm` vs `pnpm`), then generates `RESCUE_BRIEF.md` + `ONE_PROMPT_RESCUE.md`. | **~10–60 ms** local static triage · `rawEnvValuesIncluded: false` · `verifiedOutputClean: true` · `STATIC_CHECKS_PASSED` |
| **Anti-Slop Ship Gate (`anti-slop`)** | Deterministic UI & copy slop scanner (0–100 score, `Grade A+` to `F`). Strips purple radial blobs, `11.5px` fractional font sizes, emoji UI chrome, blocked mobile zoom, and locks reference-first `DESIGN.md` + `design-tokens.css` (`Linear` / `Toss` / `Stripe`). | Turned a `Grade F (0/100)` AI-slop SaaS dashboard into **`Grade A+ (100/100, 19.06:1 dark / 16.97:1 light button contrast)`** |

---

### 30-Second Quickstart

```bash
pip install git+https://github.com/ian-nocturne/deltaharness.git

# 1. Audit CLAUDE.md / AGENTS.md against your real codebase symbols
deltaharness audit . --format markdown

# 2. Pack a secret-redacted diagnostic brief & one-prompt repair plan
viberescue pack . --write

# 3. Scan AI UI slop and lock Reference-First design tokens
anti-slop scan . --format markdown
anti-slop lock . --archetype linear-precision
```

---

### House Rules at Nocturne Lab

1. **Never trust agent self-reports.** Only exit codes, verified symbol anchors, and SHA-256 digests count as done.
2. **Keep Mode by default.** If 1 file is broken, do not let the agent "refactor" 14 working files.
3. **Secret-redacted diagnostic bundles.** Diagnostic bundles record `.env` key names only (`rawEnvValuesIncluded: false`) and recursively scrub known secret token patterns (`verifiedOutputClean: true`) before handoff.
4. **Zero template slop.** 4-step integer typography, 3-step neutral mineral surfaces, 1px subtle hairlines, and zero emoji buttons.
