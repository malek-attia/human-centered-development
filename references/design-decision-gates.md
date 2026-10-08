# Design-Decision Gates

Use these gates before implementing substantial behavior. The purpose is to
discover misunderstandings and missing decisions before they become expensive
code.

## Establish the problem contract

Restate:

- requested behavior;
- invariants that must remain true;
- explicit non-goals;
- relevant current behavior;
- evidence already available; and
- unknowns that could change the design.

Do not turn an unstated assumption into architecture.

## Classify each gap

### Local and reversible

Choose a conventional implementation and continue when the decision is local,
does not change observable behavior, and can be replaced without redesigning
callers or stored state. Report it briefly at handoff.

### Evidence-dependent

Inspect documentation, source, or executable behavior before deciding. This
includes assumptions about libraries, frameworks, callbacks, retries, ordering,
serialization, and lifecycle. Record the evidence and distinguish observation
from inference.

### Architectural or semantic

Stop and return concrete alternatives to the user before implementation. This
normally includes:

- responsibility and component ownership;
- persistent state and lifecycle;
- protocol or public-contract changes;
- failure, retry, recovery, or cleanup semantics;
- correctness-versus-performance tradeoffs;
- safety fallbacks;
- handling or interpretation of user content;
- new domain vocabulary; and
- behavior that expands the requested scope.

Present only alternatives that materially differ. For each, state the behavior,
ownership impact, and tradeoff. Do not ask the user to choose among artificial
implementation details.

## Match the gate to risk

- **Small/local:** author self-review is sufficient.
- **Moderate:** provide a short problem contract and design checkpoint.
- **Sensitive/cross-cutting:** require explicit design approval, responsibility
  audit, review-sized implementation boundaries, cold-read audit, and integration
  validation.

Public contracts, recovery, concurrency, state lifecycle, interpretation of user
content, and patches to third-party internals are sensitive by default.

## Apply the authority order

Use evidence in this order:

1. explicit requirements and invariants;
2. approved architecture and vocabulary;
3. verified third-party behavior;
4. the last human-understood implementation;
5. existing code whose responsibility has been reconstructed; and
6. unreviewed prototypes or experiments.

Existing code is evidence, not automatically precedent. Do not multiply an
unclear pattern simply because it already exists.
