# Mike Ross — Internal Briefing & Thinking-Trace Document Template

Initialize this file at `<Artifact Directory Path>/scratch/mike-ross-briefing.md`
at the start of Phase 1. Keep it dense, factual, and anchored to exact
`file:line` pointers so `@main-advisor`, `@independent-reviewer`, and Phase 2
workers can jump straight to the exact spot with minimal token burn and zero
hallucination.

> **Internal Only**: Never dump this raw scratch file to the user; synthesize
> it into the clean **Case Brief** in Step 1.5.

---

```markdown
# Case Briefing & Thinking Trace: <Short Case Title>

## 1. Demand Framing & Scope (`@coordinator`)
- **User Goal**: <1–2 sentences>
- **Backward-Compatibility Constraints**: <previous-gen APIs, schemas, callers, configs>
- **Scalability & Guardrail Goals**: <growth axes, security/DB/reliability constraints>
- **Explicit Non-Goals**: <what is out of scope>
- **Repo Topology Map**: <key directories, entry points, build/test commands>

---

## 2. File Visit & Thinking-Trace Log
*(Appended by `@coordinator` as each recon slice completes. Every claim must cite exact `file:line`.)*

### Slice: <Module / Domain Name> (`worker-complex-1` / `worker-routine-1` / `@coordinator`)
- **Spot**: `<path/to/file:Lstart-Lend>` (`<SymbolName>`)
  - **Observed**: <exact factual behavior, invariant, or legacy coupling seen in code>
  - **Reasoning / Decision Impact**: <why this exact code makes us choose or rule out an option>
  - **Target Spot**: `<path/to/file:Lstart-Lend>` — <modify / wrap / preserve untouched>

---

## 3. Main Council Decision Trace (`@coordinator` + `@main-advisor`)
- **Chosen Architecture**: <concise design summary>
- **Evidence Chain**:
  - Decision 1 → justified by `<path:Lstart-Lend>` (Section 2)
  - Decision 2 → justified by `<path:Lstart-Lend>` (Section 2)
- **Backward-Compatibility & Rollback Proof**: <adapters, dual-path, rollback safety>
- **Rejected Alternatives**:
  - **Option B**: <exact evidence-backed reason rejected>
  - **Option C**: <exact evidence-backed reason rejected>
- **Main Advisor Notes** (`claude-opus-5-5-medium` / `gemini-4-carbon-medium`):
  - <blind spots caught & refinements folded in>

---

## 4. Independent Reviewer Audit Trace (`@independent-reviewer`: `gemini-4-neon-xhigh`)
### Round 1
- **Verdict**: `REVISE` | `APPROVED`
- **Critique (to Main Council)**: <exact flaws with Briefing section + `file:line` refs>
- **Main Council Resolution**: <exact change made to design & pinpoint targets>

---

## 5. Pinpoint Execution Map (For Phase 2 Workers)
*(Exact spots so execution workers edit immediately without re-scanning the repo.)*

| Task | Worker Tier | Allowed File(s) | Exact Target Spot | Contract / Definition of Done |
| :--- | :--- | :--- | :--- | :--- |
| T1 | `worker-complex-1` (`gemini-4-neon-medium`) | `<path>` | `L40-L85` (`fnName`) | <checkable outcome> |
| T2 | `worker-routine-1` (`gemini-4-hydrogen-xhigh`) | `<path>` | `L10-L25` | <checkable outcome> |
```
