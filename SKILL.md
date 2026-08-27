---
name: tencommandments
description: Read-only, on-demand code review checklist for correctness, boundaries, failures, security, and proof.
---

# $tencommandments

Use this skill only when the user explicitly asks for a Ten Commandments code
review or checklist. It is a standalone review aid, not a default workflow
gate, implementation policy, or machine-global toggle. Do not edit files,
dispatch implementation, commit, push, deploy, or change configuration while
reviewing.

## Review method

Start from the task diff and changed behavior. Read outside changed paths only
to answer a concrete correctness question raised by an applicable commandment.
Keep exploration bounded: one dependency hop per question, at most two hops
for an unresolved question, then use an already-identified authoritative
definition or report the exact unresolved question.

Apply only commandments relevant to behavior introduced or changed by the
task. Do not report unrelated pre-existing violations or invent merely
imagined failure modes.

## The Ten Commandments

1. **Ground before changing.** Establish the authoritative API, schema, type,
   config, state shape, or behavior and inspect the smallest relevant caller,
   consumer, or test.
2. **Make the smallest correct change.** Preserve unrelated behavior; avoid
   speculative cleanup, duplicate implementations, and unnecessary abstractions.
3. **Validate boundaries and realistic failure.** Handle invalid, missing,
   null, empty, stale, unexpected, timeout, disconnect, cancellation,
   unavailable-dependency, partial-result, duplicate-delivery, and resource
   limit cases when they can actually arise at a changed boundary.
4. **Bound open-ended work.** Loops, retries, pagination, queues, recursion,
   network waits, and batch sizes need an appropriate bound, termination rule,
   or backpressure.
5. **Make state transitions safe.** Preserve correctness across failure,
   retries, duplicates, and concurrency where possible; interrupted work must
   not silently corrupt or contradict state.
6. **Treat contracts as whole-system changes.** For APIs, schemas, events,
   shared types, config, persistence, or database changes, establish
   compatibility at the authoritative producer/consumer without recursively
   auditing unrelated callers.
7. **Fail gracefully and explicitly.** Realistic failures must reach a safe,
   understandable, recoverable state where possible; do not swallow errors,
   invent silent fallbacks, or turn exceptional states into success.
8. **Prove success and failure at the right layer.** Use the lowest-cost check
   that actually exercises the property. UI behavior needs UI proof,
   cross-component behavior needs integration proof, and practical bug fixes
   need regression coverage. Unexercised correctness-critical paths remain
   verification gaps.
9. **Enforce security at the authoritative boundary.** Authorization,
   privilege, secret handling, sensitive logging, data exposure, and trust
   decisions must not rely solely on client or UI convention.
10. **Make consequential changes observable and recoverable.** Meaningful
    production blast radius needs a way to determine success and a credible
    rollback, disable, retry, or recovery path. Consider recovery before any
    destructive operation.

## Report

Report findings with severity, exact file/line or behavior evidence, and a
concrete consequence. If no findings remain, report a compact `PASS` and name
important checks or verification gaps. Do not claim tests, tools, or observed
behavior without evidence from the current review.
