# Human-Centered Development Policy

Code must be optimized for human comprehension, tracing, debugging, and
contribution—not merely for automated execution.

Treat this as a standing development policy, not a one-time cleanup request.
Use the `human-centered-development` skill proactively for substantial design,
implementation, refactoring, or review work. It is especially relevant to
existing modules, stateful infrastructure, integration boundaries, failure
handling, and complex tests. Do not wait for the user to name the skill.

For an applicable task, read the skill's `SKILL.md` before acting, then read only
the references it routes to for that task. If the skill is unavailable, say so
and follow this policy without claiming to have used it. A trivial isolated
edit does not require the full workflow.

- Reconstruct current behavior before extending or moving it. Identify the
  owner, lifecycle, invariant, and callers of important state.
- Choose the smallest representation that preserves the actual contract.
  Neither extra abstractions nor fewer classes are automatically simpler.
- Name the concrete entity, fact, or responsibility. Do not make the reader
  infer it from relative roles such as producer, consumer, or source. Preserve
  established external API terms where they are accurate.
- Keep adapters and callbacks thin. Give independently changing responsibilities
  cohesive owners instead of accumulating them in a central manager.
- Verify assumptions about third-party behavior. Return material ownership,
  contract, lifecycle, and failure-policy decisions to the user when they are
  not already settled by approved requirements.
- Separate behavior-preserving refactoring from new behavior. Keep diffs focused,
  preserve unrelated work, and commit only within the authorized workflow.
- Make tests show the starting state, action, expected outcome, and failure
  evidence. Keep important scenarios directly runnable by a person.
- Before handoff, check that success and relevant failure paths can be traced
  without the development conversation. Explain conceptual changes and actual
  validation; identify remaining uncertainty.

Follow applicable project conventions and higher-priority instructions. This
policy does not grant permission to change unrelated code, redesign approved
architecture, create commits, or publish work.
