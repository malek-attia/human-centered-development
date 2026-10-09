---
name: human-centered-development
description: Design, refactor, implement, or review substantial changes to existing code so humans can understand ownership, trace control flow, debug failures, and review behavioral changes. Use for overloaded modules, sensitive infrastructure, cross-cutting features, monkey patches, recovery state machines, complex test harnesses, and agent-generated code reviews; skip trivial isolated edits.
---

# Human-Centered Development

Optimize the result for the engineer who must understand and change it later.
Correct execution is necessary but does not compensate for unclear ownership,
hidden state transitions, generic vocabulary, or an unreviewable diff.

Apply this skill proactively when the task fits its description, not only when
the user names it. Keep the process proportional: a trivial isolated edit does
not need a responsibility audit or a separate review cycle.

## Route the work

- Before substantially changing or reviewing an existing component, read
  [references/responsibility-audit.md](references/responsibility-audit.md).
- When existing structural work and new behavior are mixed, read
  [references/reviewable-change-workflow.md](references/reviewable-change-workflow.md).
- Before implementing a substantial or sensitive change, read
  [references/design-decision-gates.md](references/design-decision-gates.md).
- Before handing a sensitive or cross-cutting change to the user for review,
  read [references/cold-read-review.md](references/cold-read-review.md).
- When creating, restructuring, or reviewing tests and harnesses, read
  [references/human-readable-tests.md](references/human-readable-tests.md).

Read only the references relevant to the current task.

## Required outcomes

- A developer can identify the owner and lifecycle of each important state.
- Representations are no more elaborate than their actual contracts require.
- Repeated operations expose their domain meaning and cumulative cost as state
  grows; retained incremental state has an explicit owner and lifecycle.
- Names express the facts, responsibilities, and boundary roles callers need
  to understand, consistently in declarations and use sites.
- Name concrete entities, facts, and owned responsibilities rather than abstract
  roles whose meaning changes with context. Preserve established domain and API
  terms when they identify the concept unambiguously.
- Patch names identify the external target. Patches, adapters, callbacks, and
  external-interface boundaries remain thin.
- Distinct responsibilities have cohesive owners rather than accumulating in a
  central manager.
- The main success and failure paths are traceable without conversation history.
- Behavioral changes avoid unrelated formatting, movement, and renaming.
- Tests expose the scenario, action, expectation, and failure evidence.

Do not extract helpers merely to reduce line count. Do not create a generic
framework when a small domain-specific component is clearer. Treat file growth
as a reason to inspect responsibilities, not as an automatic violation.

Honor the user's approved vocabulary, architecture, and applicable project rules
within the governing instruction hierarchy. Preserve authorization boundaries:
prepare separate review boundaries when useful, but create commits only when
requested or already part of the authorized workflow.

Treat existing code as evidence, not automatically as an approved pattern. Do
not silently fill material design gaps: verify evidence-dependent assumptions,
and return architectural or semantic decisions to the user before implementation.

At handoff, explain what each affected module owns, how the important path flows,
what was deliberately separated, and how a human can validate the behavior.
