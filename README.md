# Ian · Nocturne Lab (`@ian-nocturne`)

> **"AI agents are great at writing 1,000 lines in 30 seconds—and terrible at telling you which 3 lines broke your build."**

Off-hours builder running **Nocturne Lab** late at night in Seoul.
I started building apps with Claude Code, Cursor, and Codex—until an agent cheerfully reported *"All fixed! 🎉"* while leaking a server key into a `"use client"` bundle, breaking my deploy, and painting the UI in unmistakable purple AI slop.

Underneath, the failure was usually the same one: the agent was acting on something that used to be true. So I build small, local-first, deterministic tools for people who ship with more than one AI agent.

> **Routlet picks the route. Memlet carries the checked context.**

---

## Memlet — agent memory you can re-check

[**Memlet**](https://github.com/ian-nocturne/memlet/tree/v9.4.0) is agent memory you can re-check, delivered when the agent touches the code it is about.

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://github.com/ian-nocturne/memlet)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-3776AB.svg)](https://github.com/ian-nocturne/memlet)
[![Zero Dependencies](https://img.shields.io/badge/Core_Deps-Zero_(Stdlib_Only)-success.svg)](https://github.com/ian-nocturne/memlet)

### The problem

Every project that uses coding agents (Claude Code, Cursor, Codex) keeps a file of rules for them,
such as `CLAUDE.md`, `AGENTS.md` or `.cursorrules`. The rules describe the code. Then the code
changes, nobody updates the rules, and the agent keeps following instructions that are no longer
true. Most memory tools make this worse in one specific way: they keep injecting a memory long
after the code it describes has changed.

### One example: when the code under a rule changes

Suppose `AGENTS.md` has two billing rules: "Always charge customers through `charge()` in
`<billing file>`." and "Refunds go through `<billing file>::refund`." Running `memlet init .`
records a baseline pin for `<billing file>::refund`, and `memlet admin anchors suggest` followed
by `memlet admin anchors accept --all-unique` anchors the first rule to `<billing file>::charge`
in `.memlet/config.json` without editing `AGENTS.md`.

Weeks later someone changes `charge(user_id, amount)` to `charge(account_id, amount, currency)`.
Without a check, an agent still reads both rules as current instructions; with Memlet, the drifted
rule is set apart everywhere an agent would read it, while the unchanged rule keeps flowing:

```console
$ memlet check --changed
STALE `<billing file>::charge` (signature_drift) — AGENTS.md:2: [suggested anchor] Always charge customers through `charge()` in `<billing file>`.
ACTIVE `<billing file>::refund` (ast_hash_match) — AGENTS.md:3: Refunds go through `<billing file>::refund`.

$ memlet recall "charge a customer"
### anchored code changed — do not apply without re-reading the rule
- [Billing] `AGENTS.md:2-2` (sha256 793dc9f2844e) [suggested anchor] [stale: signature_drift]:
  > - Always charge customers through `charge()` in `<billing file>`.

$ memlet handover . --next-step "Finish the billing refactor"
### Active constraints (anchored symbols unchanged since pinning)
- Refunds go through `<billing file>::refund`. (`AGENTS.md:3`)
### Quarantined stale anchors (not handed over)
- [suggested anchor] `<billing file>::charge`: signature_drift
```

*(Output shortened. `<billing file>` stands for a Python file under an `app` folder, and
`[suggested anchor]` marks an anchor Memlet proposed and a person accepted without editing the rule file.)*

Lessons from finished sessions follow the same pattern. `memlet learn` and `memlet dream --via claude`
propose lessons made of the exact transcript lines they came from, and nothing becomes settled
memory until a person approves it by name (`memlet learn --approve DRM-7306e549ab93 --by "robin"`).
When two lessons say the same thing in different words, `memlet dream --tidy --via claude` can
point the pair out; it only points, and a person still merges them with `memlet learn --merge`.
With opt-in Claude Code hooks (`memlet init . --hooks --guard --capsule`), an approved `BLOCKS`
prohibition attached to `<billing file>` stops the agent's first edit of that file in a session,
quotes the lesson and its citation in full, and lets the agent decide whether to repeat the call.

### What Memlet believes, the design, and what is built

Agent memory is converging on three ideas: agents should **dream** over finished sessions to keep
lessons, memories should form a typed **ontology**, and context should be **disclosed progressively**.
Memlet adopts all three under one condition:

> **A memory is only worth handing to an agent if it can be re-checked.**

Five design principles follow from that condition:

1. **Every memory has an address you can re-check.** A rule points at `file::symbol` plus a fingerprint; a lesson points at exact transcript lines (with SHA-256) and attached files; unaddressed text is labelled unverified and broken addresses are quarantined.
2. **The model proposes, code verifies, a person approves.** Deterministic code checks citations, hashes and time order; only a person, by name, can approve or merge lessons, and Memlet changes nothing on its own.
3. **Quote, never rewrite.** Rules and lessons are carried verbatim as their cited lines rather than model summaries that can drop conditions.
4. **Four relations are enough.** `GOVERNS`, `SUPERSEDES`, `RESOLVES` and `BLOCKS` join rules, lessons, symbols and files into one graph without a graph database.
5. **Send little, and say what was left out.** Anything trimmed for budget is counted rather than dropped silently, and file-scoped hooks send the memory for one file when that file is edited.

**What is actually built (9.4.0).** Everything runs locally with the Python standard library and
SQLite: no daemon and no graph database. Reads, hooks and MCP tools never launch a model; a model
is called only when you run `memlet dream --via claude` (with or without `--tidy`),
`memlet admin eval run --via claude` or the optional `memlet admin index-build`. Version 9.4.0
includes AST symbol comparison for Python (plus opt-in `tree-sitter` for TypeScript and JavaScript),
the human-approved lesson queue and four-relation graph, opt-in Claude Code hooks (`--hooks`,
`--dream`, `--jit`, `--guard`, `--capsule`), a 25-tool `memlet-mcp` server for Codex, Cursor and
other MCP clients, read-only queue tidying (`memlet admin tidy` and `memlet learn --merge`),
model-proposed same-meaning groups (`memlet dream --tidy`), and Agent Skill export
(`memlet admin skillbook --write`), with 1,160 unit tests passing on Python 3.9 and 3.13 on macOS.

### What you can do today

| Command | What it does |
| :--- | :--- |
| `memlet init` | Record baseline pins in `.memlet/`; add `--hooks` (and `--jit`, `--guard`, `--capsule`) for opt-in Claude Code hooks. |
| `memlet check` | Audit instruction files and report pinned anchors that went stale; `--changed` checks working tree drift and exits `1` when stale. |
| `memlet recall "<task>"` | Return rules with anchor status, approved lessons and labelled hints for a task or `--files`. |
| `memlet handover` | Build a checksummed context capsule of unchanged rules, approved lessons and quarantined stale anchors. |
| `memlet learn` / `memlet dream` | Queue cited lessons via keyword rules or `--via claude`, approve or merge them with `--by "robin"`, and attach files with `--path`. |
| `memlet admin graph show <file>` | Show the rules, prohibitions, fixes, replaced instructions and stale anchors attached to a file, anchor or id. |
| `memlet admin tidy` / `skillbook` | Report candidate merges (including, after `memlet dream --tidy`, what a model thinks says the same), file attachments and rule promotions without writing, or write approved lessons to `SKILL.md`. |

### What it does not do

- **Verified means unchanged, not true, and the guard does not judge edits.** Memlet checks that an anchored symbol has not changed since pinning and that a cited transcript line still says what it said; it does not check whether a rule or lesson is true. `--guard` stops the first edit of a file once per session so an approved prohibition is read in full—it cannot tell whether the edit violates it, and `--jit` context arrives with the tool result after the call ran.
- **Static analysis only, lexical TypeScript/JavaScript by default, and wording-only tidying.** Python anchors use static AST comparison (`getattr`, `exec`, `setattr` and monkeypatching are not evaluated), TypeScript and JavaScript use a lexical scan that is never `active` unless `tree-sitter` is opted in, and `admin tidy` compares word overlap rather than meaning (`dream --tidy` adds a model's opinion about meaning, which is a proposal, not a check).
- **One CLI vendor, integrity rather than authentication, and no accuracy claim.** `memlet dream` launches only the `claude` CLI under a claude.ai subscription login (`codex` and `gemini` are refused because neither documents a mode with every tool off), so the default two-vendor agreement produces no hints today. Approvals, checksums and the decision log detect edits rather than proving who acted, Linux, Windows and interactive sessions are untested, and recall quality is not measured for this release (`memlet admin eval` lets you measure it on your own sessions).

### Try it

Requires Python 3.9 or newer:

```bash
pip install git+https://github.com/ian-nocturne/memlet.git@v9.4.0

memlet init .                        # write .memlet/ baseline pins (safe to re-run)
memlet check . --format markdown     # audit instruction files and pinned anchors; exits 1 on errors
memlet recall "refund retry logic"   # rules and approved lessons for a task, with sources
```

Add `.memlet/` to your `.gitignore` unless you want to share it; the Dream queue and transcript
snapshots under it hold conversation text. To propose anchors for rules that mention functions
without `file::symbol` syntax, run `memlet admin anchors suggest`; to preview Claude Code hooks
without modifying settings, run `memlet init . --hooks --dry-run`.

### Read more

Version 9.4.0 lives at **[github.com/ian-nocturne/memlet, tag `v9.4.0`](https://github.com/ian-nocturne/memlet/tree/v9.4.0)** (Apache-2.0).

Its README walks through session dreaming and the four relations (`Example 2`), the guarded first
edit (`Example 3`), and merging repeated lessons into an Agent Skill (`Example 4`). It also
includes the full *From principle to code* table mapping each principle to what is built in 9.4
and what is weaker than it sounds, setup details for Claude Code hooks and `memlet-mcp`, the
GitHub Action, and the sealed `memlet admin eval` memory experiment.

---

## Routlet — which model for which job

Evidence-based multi-model routing and supervision skill. It refreshes dated evidence about models, checks what the host can actually run, proposes performance / balanced / efficiency plans, and supervises the one the user picks. It recommends; it never inherits execution or billing approval across vendors.

---

### House rules at Nocturne Lab

1. **Never trust agent self-reports.** Only exit codes, checked code anchors, and SHA-256 digests count as done.
2. **A memory is only as good as the code it points to.** If the anchored symbol changed, the memory is `stale` until someone re-reads it.
3. **Keep Mode by default.** If 1 file is broken, do not let the agent "refactor" 14 working files.
