---
name: tencommandments
description: Read-only, on-demand code review against the latest commits, current diff, commits of the day, or the entire codebase.
---

# $tencommandments

Use this skill only when the user explicitly asks for an on-demand code review
against one of these scopes: the latest commits, the current diff, commits of
the day, or the entire codebase. If the user does not name a scope, ask them to
choose one. It is a standalone review aid, not a default workflow gate,
implementation policy, or machine-global toggle. Do not edit files, dispatch
implementation, commit, push, deploy, or change configuration while reviewing.

Resolve the requested review scope from authoritative repository state before
reviewing. For the current diff, inspect the working-tree and staged diff. For
latest commits, inspect the newest relevant commits and their diffs. For
commits of the day, inspect commits whose repository timestamps fall on the
current calendar day. For the entire codebase, inspect the repository layout,
entry points, high-risk boundaries, and relevant tests, then report any
coverage limits instead of implying exhaustive proof.

## Review method

Start from the selected commits or diff and changed behavior. Read outside
changed paths only to answer a concrete correctness question raised by an
applicable commandment. Keep exploration bounded: one dependency hop per
question, at most two hops for an unresolved question, then use an
already-identified authoritative definition or report the exact unresolved
question.

Apply only commandments relevant to behavior introduced or changed by the
task. Do not report unrelated pre-existing violations or invent merely
imagined failure modes.

## The Ten Commandments

This section is the canonical rule data for consumers such as EngineeringBar.
Reading or enforcing these rules does not invoke this explicit review, and
prior implementation enforcement does not establish that an independent
review passes.

1. **Ground before changing.** Establish the authoritative API, schema, type,
   config, state shape, behavior, and relevant ownership boundary. Inspect the
   smallest relevant caller, consumer, implementation, or test before
   modifying it. Do not infer contracts from names or nearby code when an
   authoritative source exists.
2. **Make the smallest coherent correct change.** Preserve unrelated behavior
   and avoid speculative cleanup, duplicate implementations, and unnecessary
   abstractions. Keep responsibilities cohesive: if a change materially
   worsens an already overloaded function, module, component, or class, extract
   the smallest meaningful boundary rather than adding another responsibility
   to it.
3. **Write code for the next reader.** Names should communicate intent; control
   flow should be easy to follow; functions and units should operate at a
   coherent level of abstraction. Prefer clear, unsurprising code over cleverness.
   Comments should explain constraints, decisions, or why something exists—not
   translate unnecessarily obscure code.
4. **Keep responsibilities and dependencies disciplined.** A unit should have
   a clear reason to change. Separate unrelated policy, orchestration, I/O,
   persistence, presentation, and domain logic when doing so creates a real
   boundary. Depend on stable contracts rather than implementation details, and
   avoid unnecessary coupling, hidden global state, and knowledge of distant
   internals.
5. **Validate boundaries and realistic failure.** Handle invalid, missing,
   null, empty, stale, unexpected, timeout, disconnect, cancellation,
   unavailable-dependency, partial-result, duplicate-delivery, and resource-limit
   cases when they can actually arise at a changed boundary. Validate at the
   layer responsible for enforcing the invariant.
6. **Bound work and make state transitions safe.** Loops, retries, pagination,
   queues, recursion, network waits, and batch sizes need appropriate bounds,
   termination rules, or backpressure. Preserve correctness across failure,
   retries, duplicates, interruption, and concurrency; work must not silently
   corrupt or contradict state.
7. **Treat contracts as whole-system changes.** APIs, schemas, events, shared
   types, config, persistence, and database changes require compatibility at
   their authoritative producers and consumers. Keep interfaces narrow and
   explicit, avoid leaking implementation details, and do not recursively audit
   unrelated callers without evidence that the contract requires it.
8. **Fail explicitly and prove behavior at the right layer.** Realistic
   failures must reach a safe, understandable, recoverable state where possible.
   Do not swallow errors, invent silent fallbacks, or turn exceptional states
   into success. Use the lowest-cost test that actually exercises the property:
   unit for isolated logic, integration for cross-boundary behavior, UI/E2E for
   user-visible behavior, and regression coverage for practical bugs.
9. **Enforce trust and invariants at authoritative boundaries.** Authorization,
   privilege, secret handling, sensitive logging, data exposure, validation,
   and other trust decisions must not rely solely on UI or client convention.
   Important domain invariants should have one authoritative enforcement point
   rather than duplicated assumptions scattered across the codebase.
10. **Make consequential changes maintainable, observable, and recoverable.**
    Meaningful production blast radius needs a way to determine success and a
    credible rollback, disable, retry, or recovery path. Avoid leaving newly
    touched code harder to understand than necessary: remove duplication
    introduced by the change, preserve useful abstractions, and address
    structural debt when the change materially aggravates it—but do not turn
    scoped work into an unrelated rewrite.

## Report

Report findings with severity, exact file/line or behavior evidence, and a
concrete consequence. If no findings remain, report a compact `PASS` and name
important checks or verification gaps. Do not claim tests, tools, or observed
behavior without evidence from the current review.
