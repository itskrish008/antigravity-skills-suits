# Suits — Antigravity Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Platform: Google Antigravity](https://img.shields.io/badge/Platform-Google%20Antigravity-4285F4.svg)](https://antigravity.google)

A collection of **Google Antigravity** skills inspired by the TV series ***Suits*** — starting with **Mike Ross**, a whole-codebase architect and token-efficient multi-model swarm orchestrator.

---

## Available Skills

### Mike Ross (`mike-ross`) — Whole-Codebase Architect & Multi-Model Swarm Orchestrator

Modeled after Harvey Specter's eidetic associate, **Mike Ross** reads the entire relevant codebase before touching a single line of code, uncovers hidden precedents and legacy traps, protects backward compatibility across previous generations, and designs a future-scalable solution vetted by an independent reviewer.

#### Why Use Mike Ross?
- **Read-Only First (Two-Phase Gate)**: Never edits code prematurely. Phase 1 delivers a crisp, evidence-backed **Case Brief** (precedents, backward-compatibility & rollback audit, rejected alternatives, and a file-level blueprint). Phase 2 executes only after your approval.
- **Shared Thinking-Trace Briefing Document**: Every file visited is logged once in an internal scratch briefing (`<Artifact Directory>/scratch/mike-ross-briefing.md`) with exact `file:line` anchors, observed invariants, causal reasoning, and pinpoint target spots — drastically cutting token burn and eliminating hallucinations.
- **Main Council + Single Independent Reviewer Loop**: Mike Ross (`gemini-4-neon-high`) collaborates with a Main Council advisor (`claude-opus-5-5-medium`, falling back to `gemini-4-carbon-medium`), then passes the full Thinking-Trace and architecture to a single independent **`gemini-4-neon-xhigh`** reviewer who critiques directly to the Main Council until approved.
- **Strict Sub-Agent Discipline**: Complex and moderate tasks run on `gemini-4-neon-medium`, boilerplate and routine sweeps run on `gemini-4-hydrogen-xhigh`, **Gemini Argon is strictly banned**, and sub-agents follow strict hub-and-spoke isolation.

---

## Architecture & Multi-Model Flow

```text
User <===> Main Council
           ├── Mike Ross (@coordinator: gemini-4-neon-high)  [Sole user-facing orchestrator]
           └── @main-advisor: claude-opus-5-5-medium
                              (fallback: gemini-4-carbon-medium if Opus 5.5 Medium unavailable)
                    │
                    ├── Shared Internal Thinking-Trace Document
                    │   <Artifact Directory>/scratch/mike-ross-briefing.md
                    │   (Files visited, exact file:line observations, decision trace, pinpoint spots)
                    │
                    ├── Sub-Agent Council (STRICT hub-and-spoke — NO worker-to-worker chat)
                    │   ├── worker-complex-* (gemini-4-neon-medium)    [Complex & moderate tasks]
                    │   └── worker-routine-* (gemini-4-hydrogen-xhigh) [Boilerplate, routine, web/sweeps]
                    │   ❌ STRICTLY BANNED: ALL Gemini Argon models
                    │
                    └── Independent Reviewer (SINGLE agent — reads Briefing Doc directly)
                        └── independent-reviewer (gemini-4-neon-xhigh) [Exactly 1]
                              └── Sends critique directly to Main Council
                                  (@coordinator + @main-advisor) until APPROVED
```

| Tier | Roster Name | Exact Model ID | Role | Cap |
| :--- | :--- | :--- | :--- | :--- |
| **Main Council** | `coordinator` (Mike Ross) | `gemini-4-neon-high` | Sole user interface, demand framing, decomposition, Briefing Doc owner, synthesis, verification | 1 |
| **Main Council** | `main-advisor` | `claude-opus-5-5-medium` *(fallback: `gemini-4-carbon-medium`)* | High-signal architectural partner; audits Thinking Trace & co-resolves reviewer feedback | 0–1 |
| **Sub-Agent** | `worker-complex-*` | `gemini-4-neon-medium` | Complex & moderate work: databases, security, guardrails, architecture, compatibility, tests | Dynamic |
| **Sub-Agent** | `worker-routine-*` | `gemini-4-hydrogen-xhigh` | Boilerplate, routine edits, rapid repo sweeps, caller inventories, web/docs lookups | Dynamic |
| **Independent Reviewer** | `independent-reviewer` | `gemini-4-neon-xhigh` | Sole independent critic; audits Briefing Doc & critiques directly to Main Council | **1** |

---

## Installation

Choose whichever installation method fits your workflow:

### Option 1: Install Globally as an Antigravity Plugin *(Recommended)*

Installing as a plugin bundles **both** the `mike-ross` skill (`/mike-ross`) and the always-on `"Hey Mike"` trigger rule (`rules/AGENTS.md`) automatically across all workspaces.

**Using the Antigravity CLI (`agy`):**
```bash
agy plugin install https://github.com/thekrish008/antigravity-skills-suits
```

**Or clone directly into your global plugins directory:**

- **macOS / Linux:**
  ```bash
  git clone https://github.com/thekrish008/antigravity-skills-suits.git ~/.gemini/config/plugins/antigravity-skills-suits
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/thekrish008/antigravity-skills-suits.git "$HOME\.gemini\config\plugins\antigravity-skills-suits"
  ```

---

### Option 2: Install Globally as a Standalone Skill

Copy `skills/mike-ross/` into your global Antigravity skills folder:

- **macOS / Linux:**
  ```bash
  git clone https://github.com/thekrish008/antigravity-skills-suits.git /tmp/antigravity-skills-suits
  mkdir -p ~/.gemini/config/skills
  cp -r /tmp/antigravity-skills-suits/skills/mike-ross ~/.gemini/config/skills/
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/thekrish008/antigravity-skills-suits.git "$env:TEMP\antigravity-skills-suits"
  New-Item -ItemType Directory -Force -Path "$HOME\.gemini\config\skills" | Out-Null
  Copy-Item -Recurse -Force "$env:TEMP\antigravity-skills-suits\skills\mike-ross" "$HOME\.gemini\config\skills\"
  ```

**(Optional — Guaranteed `"Hey Mike"` Trigger):** Append the contents of [`rules/AGENTS.md`](./rules/AGENTS.md) to `~/.gemini/config/GEMINI.md` so starting any conversation with `"Hey Mike"` always triggers the skill deterministically.

---

### Option 3: Install for a Single Project (Workspace Scope)

Copy `skills/mike-ross/` into `.agents/skills/mike-ross/` inside your project repository and commit it so your whole team gets it automatically:

```bash
mkdir -p .agents/skills
cp -r path/to/antigravity-skills-suits/skills/mike-ross .agents/skills/
```

---

## How to Use

Open any conversation in Antigravity and start with either the slash command or natural greeting:

- **Slash command:**
  ```text
  /mike-ross Analyze our authentication and session layer and design a zero-downtime migration to multi-tenant RBAC.
  ```
- **Natural-language trigger:**
  ```text
  Hey Mike, inspect the entire billing module and propose the cleanest, backward-compatible way to support usage-based pricing at 10x scale.
  ```

### What Happens Next
1. **Phase 1 (Read-Only Analysis & Review)**: Mike switches to `gemini-4-neon-high`, maps the repo, dispatches lean non-overlapping recon sub-agents to build the internal Thinking-Trace Briefing Document, consults `main-advisor`, passes the design through `independent-reviewer` (`gemini-4-neon-xhigh`) until `VERDICT: APPROVED`, and presents one structured **Case Brief** (`The Brief`, `Codebase Precedents & Hidden Constraints`, `Backward-Compatibility & Migration Audit`, `The Winning Strategy`, and `File-Level Implementation Blueprint`).
2. **Approval Gate**: Mike pauses and asks whether you want to proceed, adjust any trade-off, or stop at the blueprint.
3. **Phase 2 (Pinpoint Execution)**: Once approved, workers jump straight to the exact `file:line` targets documented in the Briefing Document, Mike inspects every diff and runs tests himself, cleans up workers, and delivers one synthesized summary.

---

## Repository Structure

```text
antigravity-skills-suits/
├── plugin.json                                     # Antigravity plugin manifest
├── README.md                                       # Documentation & install guide
├── LICENSE                                         # MIT License
├── .agents/
│   └── skills.json                                 # Local workspace auto-discovery
├── rules/
│   └── AGENTS.md                                   # "Hey Mike" / "/mike-ross" trigger rule
└── skills/
    └── mike-ross/
        ├── SKILL.md                                # Main Mike Ross skill playbook
        └── references/
            ├── briefing-template.md                # Shared Thinking-Trace Briefing Doc template
            └── audit-checklists.md                 # Precedent, compatibility, scaling & security lenses
```

---

## Roadmap — The Suits Cast

| Character | Specialty | Status |
| :--- | :--- | :--- |
| **Mike Ross** (`mike-ross`) | Whole-codebase research, architectural blueprint, compatibility & scaling, multi-model swarm orchestrator | Available |
| **Harvey Specter** | High-stakes executive closer & decisive end-to-end delivery | Planned |
| **Louis Litt** | Meticulous code, security, performance & test-coverage auditor | Planned |
| **Donna Paulsen** | Context, memory, communication & workflow anticipation | Planned |
| **Jessica Pearson** | Release governance, PR sign-off & cross-repo strategy | Planned |

---

## Contributing

Issues, PRs, and ideas for new *Suits* skills or refinements to `mike-ross` are welcome! Please keep skills aligned with the [Google Antigravity Skills specification](https://antigravity.google/docs/skills.md).

## License

MIT © 2026 Krish — see [LICENSE](./LICENSE).
