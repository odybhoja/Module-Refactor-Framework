# Module Refactor Framework

A step-by-step approach for planning and sequencing a module refactor safely, with guidance on what to test at each stage.

## 1. Establish a Safety Net Before Touching Anything

- **Characterize existing behavior with tests.** If coverage is thin, write characterization tests first — tests that lock in *current* behavior (even quirky behavior), not the behavior you wish it had.
- **Identify all callers/consumers** of the module (internal imports, public API consumers, config-driven usage). Grep the codebase, check for reflection/dynamic imports, and check external docs/API contracts if it's a published module.
- **Snapshot current metrics**: performance benchmarks, error rates, memory footprint — anything you might regress without noticing functionally.
- **Commit to a branch strategy**: small, revertible commits, feature-flagged if the refactor is large or touches production-critical paths.

## 2. Map the Boundary Before the Internals

- Define the module's **public interface** (what other code depends on) separately from its **internal implementation** (what you're free to change).
- Write down the contract: inputs, outputs, side effects, error conditions, invariants. This becomes your test oracle later.
- If the interface itself needs to change, treat that as a *separate* refactor phase from internal restructuring — don't change the "what" and "how" simultaneously.

## 3. Sequence Changes From Low-Risk to High-Risk

A safe ordering, roughly:

1. **Non-behavioral cleanup** (renaming, formatting, dead code removal, comment updates) — zero logic change, verified by diff review alone.
2. **Structural extraction** (splitting functions/classes, extracting helpers) — behavior-preserving, verified by existing tests passing unchanged.
3. **Dependency inversion / interface introduction** (introducing seams, abstracting external dependencies) — enables future changes without changing current behavior.
4. **Internal logic changes** (algorithm swaps, data structure changes) — the actual "risky" part, done last and in isolation once the above have de-risked everything else.
5. **Interface/contract changes** (if needed) — done separately, with a migration path for callers (deprecation warnings, adapters, versioning).

> **Principle:** each step should be independently revertible, and you should never mix a refactor step with a behavior-change step in the same commit.

## 4. What to Test at Each Stage

| Stage | What to Verify |
|---|---|
| Non-behavioral cleanup | Diff review; test suite passes unchanged; no new test needed |
| Structural extraction | Existing tests still pass; add unit tests for newly-extracted units if they lacked direct coverage |
| Dependency inversion | Existing tests pass via the new seam; add a test double/mock to confirm the seam is actually swappable |
| Logic changes | Characterization tests still pass; add new tests for edge cases the old structure made hard to test; property-based tests if the logic has invariants |
| Interface changes | Contract tests against the new interface; integration tests with real callers; backward-compat tests if old interface is still supported temporarily |

Additionally, at every stage:

- **Run the full test suite**, not just tests "related" to the change — refactors have surprising blast radii.
- **Re-check the metrics snapshot** from step 1 after each stage, not just at the end.
- **Diff the actual behavior**, not just "tests pass" — for critical modules, consider running old and new implementations side-by-side (shadow mode) on real traffic/data before fully cutting over.

## 5. Rollout and Cutover

- If callers exist outside your immediate control, use an **adapter or shim layer** so old and new interfaces coexist temporarily.
- Roll out behind a **feature flag** if risk is nontrivial, so you can toggle back without a code revert.
- Remove the old code path only after a defined bake-in period with no regressions, and after confirming no lingering callers via usage telemetry or a deprecation-warning period.
- Document what changed and why — especially any behavior that changed even subtly (error message wording, timing, ordering guarantees) since those are the regressions that don't show up in unit tests but break downstream consumers.

## Key Failure Modes to Watch For

- **Mixing refactor and feature work** in the same change — makes it impossible to tell what broke.
- **Trusting green tests too much** when the tests were written against the *new* structure rather than characterizing old behavior first.
- **Big-bang cutovers** on modules with many hidden/implicit consumers.
- **Skipping the "map the boundary" step** and discovering mid-refactor that some other part of the codebase relies on an internal detail you assumed was private.
