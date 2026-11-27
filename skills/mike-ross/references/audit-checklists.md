# Mike Ross — Deep Audit Checklists

Load this reference during **Phase 1 (Discovery & Case Brief)** whenever a task
touches non-trivial legacy surfaces, shared contracts, persistence, or
long-term scaling boundaries.

---

## 1. Codebase Precedent & Hidden-Constraint Lenses

Before proposing a new pattern, verify what the codebase *already* established:

- **Canonical Patterns**: How does the repo already handle errors, logging,
  dependency injection, configuration, async work, retries, and testing? Match
  existing idioms unless there is a compelling, explicitly justified reason to
  introduce a new one.
- **Hidden Couplings**:
  - Implicit initialization order, singletons, module-level side effects, or
    global caches.
  - Reflection, code generation, dynamic imports, barrel files, or convention-
    based routing/loading.
  - Shared mutable state, database triggers, event listeners, or background
    cron/queue consumers outside the obvious call graph.
- **Historical Context**: Check git history, comments, ADRs, and deprecated
  wrappers to understand *why* awkward code exists — never delete a "weird"
  guard without proving the bug it prevented is gone.

---

## 2. Backward-Compatibility & Previous-Gen Audit Matrix

Evaluate every surface that previous-generation clients, deployments, or data
may still rely on:

| Surface | Failure Modes to Check | Safe Migration Patterns |
| :--- | :--- | :--- |
| **Public / Internal APIs** | Removed/renamed fields, stricter input validation, changed error codes, altered defaults or ordering | Additive changes first; versioned routes/negotiation; thin compatibility adapter delegating to new core; deprecation warnings before removal |
| **Database & Stored Data** | Destructive column drops/renames, NOT NULL without backfill, enum narrowing, incompatible JSON/blob formats | Expand-and-contract (add nullable → dual-write → backfill → switch reads → contract in later release); tolerant readers for legacy payloads |
| **Events, Queues & Caches** | Mixed-version producers/consumers during rolling deploys; stale cached shapes breaking new code | Versioned/key-prefixed cache entries; forward- and backward-compatible envelopes; ignore unknown fields cleanly |
| **Config, Env & CLI Flags** | Renamed env vars/flags, changed config schema defaults, breaking exit codes or stdout formats | Support legacy keys as aliases with precedence rules and clear deprecation notices |
| **SDK / Library Contracts** | Changed function signatures, return types, thrown exceptions, side-effect timing, peer dependency bumps | Preserve legacy overloads/wrappers; keep default behavior unchanged; opt-in flags for breaking semantics |

### Rollback Verification
Always ask: *"If we deploy this and roll back the binary 20 minutes later, will
data written during those 20 minutes still be readable by the previous version?"*
If not, redesign the rollout as multi-step expand-and-contract.

---

## 3. Future Scalability & Extensibility Lenses

Evaluate whether the proposed solution ages cleanly without premature
over-engineering:

- **Data & Traffic Growth (10x–100x)**:
  - Check for N+1 queries, unbounded in-memory collections, full-table scans,
    missing pagination/streaming, lock contention, or hot partitions.
  - Check payload amplification, fan-out storms, and retry cascades.
- **Architectural Extension Points**:
  - Isolate volatile rules/integrations behind narrow, cohesive interfaces so
    future variants plug in without editing core orchestration logic.
  - Keep state ownership unambiguous — single source of truth per domain entity.
- **Operational & Team Scalability**:
  - Clear module boundaries so multiple engineers/agents can evolve adjacent
    features without merge conflicts or tight coupling.
  - Built-in observability hooks (structured errors, metrics/logging points)
    where failures would otherwise be silent.
- **Anti-Over-Engineering Check**:
  - Prefer *reversible*, cleanly bounded abstractions over speculative
    frameworks. Design easy doors for tomorrow without building rooms nobody
    needs today.

---

## 4. Security, Database & Guardrail Lenses (`worker-complex-*` Priority)

Whenever a change touches persistence, auth, external inputs, or safety
boundaries, audit these before finalizing the architecture:

- **Database & Data Integrity**:
  - Transaction boundaries, isolation levels, race conditions (TOCTOU, double
    writes, missing idempotency keys), lock escalation, and index coverage on
    hot queries.
  - Zero-downtime migration safety (avoid full-table locks on large tables).
- **Security & Trust Boundaries**:
  - Authentication vs. authorization (tenant/object-level access checks at
    every entry point, never trusting client-supplied IDs or roles).
  - Secret handling, injection surfaces (SQL/command/template/SSRF), and
    safe error messages (no sensitive leakage to callers).
- **Guardrails & Failure Containment**:
  - Rate limits, payload/size bounds, timeouts, circuit breakers, and explicit
    validation at system edges so bad inputs fail closed rather than silently
    corrupting state.

