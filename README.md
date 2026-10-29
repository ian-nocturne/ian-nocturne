# Ian · Nocturne Lab (`@ian-nocturne`)

> **Category sample:** the public-repository numbers below come from 4 repositories pinned at fixed commits (`vercel/ai`, `openai-agents-python`, `AutoGPT`, `claude-code`; 784 docs, 2,745 references, 10 findings). They are not a guarantee for other repositories. "0 false positives" means the 10 findings in this sample were checked by hand.

> **"AI agents are great at writing 1,000 lines in 30 seconds—and terrible at telling you which 3 lines broke your build."**

Off-hours builder running **Nocturne Lab** late at night in Seoul.
I started building apps with Claude Code, Cursor, and Codex—until an agent cheerfully reported *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle, breaking my deploy, and painting the UI in unmistakable purple AI slop.

Now I build small, local-first, deterministic infrastructure for builders who ship with more than one AI agent: the **`-let` suite**.

> **Routlet picks the route. Memlet carries the verified context.**

---

### The `-let` Agent Infrastructure Suite

| Project | What It Does | Proof / Boundaries |
|---|---|---|
| **[Memlet (`memlet`)](https://github.com/ian-nocturne/deltaharness)**<br>*formerly DeltaHarness / Ieum Memory* | **Agent memory that checks whether what it remembers is still true.** Code-anchored, bi-temporal memory and instruction drift guard for coding agents:<br>• Wiki + ledger: `memlet init` writes `.memlet/config.json`, `.memlet/wiki/INDEX.md`, and a local `.memlet/ledger.sqlite3`; `memlet wiki` records human edits to `.memlet/wiki/*.md` as `user_edit` events and stamps each page with an AST status header<br>• AST & signature contract guard: every `file::Symbol` anchor is labeled `active` / `compatible-unverified` (body changed or optional argument added) / `stale` (symbol gone or required arguments changed)<br>• `memlet import`: pulls rules from `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `.cursor/rules/*.mdc`, `.ai-memory/`, `.claude-mem/` and tags each with its live anchor status<br>• `memlet handover`: signed, <500-token capsule for the next model; only `active` constraints travel, `stale` anchors are quarantined, dead-ends ride along as rejected approaches<br>• Session Delta-Prompting (send only what changed when `coreHash` is unchanged) + 2-Hop PPR recall<br>• 5-factor admission with poisoning quarantine (utility and source trust scored separately)<br>• `memlet check`: static drift audit for `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `SKILL.md`<br>• `memlet ui`: Nocturne Memory & Routing Inspector, one self-contained HTML page (AST health, benchmark, handover, Routlet matrix)<br>• `memlet pack`: Routlet × Memlet Pro Harness Pack for `nextjs-supabase` / `fastapi-postgres` / `swift-ios` (7 files per stack, SHA-256 manifest) | `memlet check` audited **784 docs / 2,745 refs** across `vercel/ai`, `openai-agents-python`, `AutoGPT`, & `claude-code` · **10 real broken paths caught, 0 false positives in this category sample** · zero external LLM calls on the default path |
| **Routlet (`routlet`)** | Evidence-based multi-model routing & supervision skill. Refreshes dated model evidence, checks what the host can actually run, proposes performance / balanced / efficiency plans, then supervises the orchestration the user picks. | Recommends; does not inherit execution or billing approval across vendors. Evidence is dated and refreshed, not assumed. |
| **VibeRescue (`viberescue`)**<br>*Memlet sub-plugin* | Secret-redacted (`SECRET_REDACTED_BUNDLE`) triage & handoff bundler for stuck vibe-coded apps. Flags missing `.env` keys, exposed `"use client"` secrets, missing Supabase RLS, and competing lockfiles (`npm` vs `pnpm`), then generates `RESCUE_BRIEF.md` + `ONE_PROMPT_RESCUE.md`. | **~10–60 ms** local static triage · `rawEnvValuesIncluded: false` · `verifiedOutputClean: true` · `STATIC_CHECKS_PASSED` is a static result, not a live production guarantee |
| **Anti-Slop Ship Gate (`anti-slop`)**<br>*Memlet sub-plugin* | Deterministic UI & copy slop scanner (0–100 score, `Grade A+` to `F`). Strips purple radial blobs, `11.5px` fractional font sizes, emoji UI chrome, blocked mobile zoom, and locks reference-first `DESIGN.md` + `design-tokens.css` (`Linear` / `Toss` / `Stripe`). | Turned one AI-slop SaaS dashboard (dogfood sandbox) from cleanliness `0/100 (Grade F)` into **`100/100 (Grade A+)`** with 19.06:1 dark / 16.97:1 light button contrast · static checks, single case |

---

### 30-Second Quickstart

```bash
pip install git+https://github.com/ian-nocturne/deltaharness.git

# 1. One-command, zero-daemon setup (.memlet/config.json + .memlet/ledger.sqlite3 + .memlet/wiki/INDEX.md)
memlet init .

# 2. Audit CLAUDE.md / AGENTS.md against your real codebase symbols (alias: audit)
memlet check . --format markdown

# 3. Re-check wiki anchors, refresh per-page AST status headers, and sync page edits into the ledger
memlet wiki .

# 4. Import existing CLAUDE.md / .cursorrules / .ai-memory rules, then hand over only what still checks out
memlet import . --dry-run --format markdown
memlet handover . --next-step "Finish the refund refactor" --rejected "Retry inside webhook" --format markdown

# 5. Pack a secret-redacted diagnostic brief & one-prompt repair plan (or: memlet rescue .)
viberescue pack . --write

# 6. Scan AI UI slop and lock reference-first design tokens (or: memlet anti-slop .)
anti-slop scan . --format markdown

# 7. Nocturne Memory & Routing Inspector: one self-contained, escaped HTML page
#    (AST anchor health, benchmark, handover capsule, Routlet matrix)
memlet ui .    # writes .memlet/inspector.html (or --output FILE)

# 8. Routlet × Memlet Pro Harness Pack for nextjs-supabase / fastapi-postgres / swift-ios
#    CLAUDE.md, AGENTS.md, SKILL.md, PITFALLS.md, Routlet preset, design tokens, SHA-256 MANIFEST.json
memlet pack --stack all --output ./harness-packs
```

Legacy command names `ieum-memory`, `deltaharness`, and `prism-memory` remain supported as aliases.

---

### Refactoring & Handover Benchmark (reproducible, 0 LLM calls)

`memlet benchmark --format markdown` replays 4 small refactors in a temp directory (breaking signature change, optional argument added, file move, cross-vendor handover with 2 recorded dead-ends) and prints the same `resultDigest` on every run.

| Mode | Stale procedures injected | Optional-arg false positives | Handover hazards prevented |
|---|:---:|:---:|:---:|
| Raw Markdown (static `CLAUDE.md` / `.cursorrules`) | 100% (3/3) | 0% (0/1) | 0% (0/3) |
| Unverified Wiki (timestamps, aging, file-exists check) | 66.7% (2/3) | 0% (0/1) | 66.7% (2/3) |
| Memlet (AST + `reqSigHash` + handover quarantine) | **0% (0/3)** | **0% (0/1)** | **100% (3/3)** |

Four synthetic fixtures built to exercise these specific failure modes; "Unverified Wiki" is a simplified model, not a run of any particular product.

---

### House Rules at Nocturne Lab

1. **Never trust agent self-reports.** Only exit codes, verified symbol anchors, and SHA-256 digests count as done.
2. **A memory is only as good as the code it points to.** If the anchored symbol changed, the memory is `stale` until re-verified.
3. **Keep Mode by default.** If 1 file is broken, do not let the agent "refactor" 14 working files.
4. **Secret-redacted diagnostic bundles.** Diagnostic bundles record `.env` key names only (`rawEnvValuesIncluded: false`) and recursively scrub known secret token patterns (`verifiedOutputClean: true`) before handoff.
5. **Zero template slop.** 4-step integer typography, 3-step neutral mineral surfaces, 1px subtle hairlines, and zero emoji buttons.
