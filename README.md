# Ian · Nocturne Lab (`@ian-nocturne`)

> **"AI coding agents write fast, lie confidently, and paint everything in purple radial gradients. I build deterministic harnesses that keep them honest."**

Off-duty builder running **Nocturne Lab** late at night in Seoul.
I started as a vibe coder until an agent claimed *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle and breaking my production deploy at 2 AM. Now I build zero-dependency, surgical verification tools for people who ship with Claude Code, Cursor, and Codex.

---

### What I Build

| Project | What It Does | Speed / Proof |
|---|---|---|
| **[DeltaHarness](https://github.com/ian-nocturne/deltaharness)** | Zero-setup polyglot AST auditor for `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, and `SKILL.md`. Catches broken paths, renamed `npm/pnpm` scripts, and function signature drift before your agent hallucinates. | **12–230 ms** scan · Caught 5 real drifts in `vercel/ai` & 1 in `anthropics/claude-code` |
| **VibeRescue (`viberescue`)** | Secret-safe (`ZERO_SECRET_VALUES_INCLUDED`) diagnostic bundler for stuck vibe-coded apps. Flags missing `.env` keys, exposed `"use client"` secrets, missing Supabase RLS, and competing lockfiles (`npm` vs `pnpm`), then generates `RESCUE_BRIEF.md` + `ONE_PROMPT_RESCUE.md`. | **< 10 ms** local pack · `rawEnvValuesIncluded: false` |
| **Anti-Slop Ship Gate (`anti-slop`)** | Deterministic UI & copy slop scanner (0–100 score, `Grade A+` to `F`). Strips purple radial blobs, `11.5px` fractional font sizes, emoji UI chrome, blocked mobile zoom, and locks reference-first `DESIGN.md` + `design-tokens.css` (`Linear` / `Toss` / `Stripe`). | Turned a 150-violation `Grade F` dashboard into **`Grade A+ (100/100)`** |

---

### 30-Second Quickstart

```bash
pip install git+https://github.com/ian-nocturne/deltaharness.git

# 1. Audit CLAUDE.md / AGENTS.md against your real codebase AST
deltaharness audit . --format markdown

# 2. Pack a secret-safe diagnostic brief & one-prompt rescue for a broken build
viberescue pack . --write

# 3. Score your UI for AI Slop and lock Reference-First design tokens
anti-slop scan . --format markdown
anti-slop lock . --archetype linear-precision
```

---

### House Rules at Nocturne Lab

1. **Never trust an agent's self-report.** Only exit codes, AST symbol anchors, and SHA-256 digests count as done.
2. **Surgical fixes over full rewrites (`Keep Mode`).** If 1 file is broken, do not let the agent "refactor" 14 working files.
3. **Zero secret values leave the machine.** Diagnostic bundles record key names and structural diffs only (`strict_keys_only`).
4. **No AI template slop.** 4-step integer typography, 3-step neutral mineral surfaces, 1px subtle hairlines, and zero emoji buttons.
