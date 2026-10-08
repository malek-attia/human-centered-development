# Human-Readable Tests

Tests are operational documentation. A test is not acceptable merely because an
agent can generate and execute it.

## Make the scenario visible

A developer reading the test should immediately find:

1. the starting state;
2. the action or injected failure;
3. the expected behavior; and
4. the evidence shown when the expectation fails.

Use scenario names and domain-specific helpers. Avoid a single generic harness
driven by nested dictionaries, callback tables, and flags that hide the story.

## Make it runnable

Document the direct command for important suites and end-to-end scenarios. Keep
environment requirements and intentionally long waits explicit. Do not silently
skip the behavior that makes a test slow or difficult; if it belongs in the
suite, make the waiting observable.

A developer should be able to:

- run one scenario independently;
- place a breakpoint at the injected event and assertion;
- see component logs associated with the run; and
- distinguish an infrastructure setup failure from a behavior failure.

## Organize by purpose

Keep categories semantically distinct:

- **Unit:** one state machine, transformation, or algorithm.
- **Correctness:** public behavior through the real component surface.
- **Component fault tolerance:** failures in project-owned components.
- **Engine contracts:** verified assumptions about third-party execution.
- **End to end:** cross-component production-like scenarios.
- **Experiments:** temporary investigations and proofs, not guarantees.

Do not promote design invariants into artificial test categories when they are
better enforced through architecture and code review.

## Prefer evidence over framework cleverness

Assertions should identify domain objects and expected transitions. Failure
messages and logs should answer what happened without requiring the reader to
reverse-engineer the harness. Reuse setup when it removes noise, but keep the
scenario's meaningful actions in the test body.

Before finishing, ask whether a teammate could diagnose a failed run without the
test author's help. If not, simplify the harness or improve the visible evidence.
