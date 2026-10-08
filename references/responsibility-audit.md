# Responsibility Audit

Use this audit before adding substantial behavior to an existing component or
when reviewing code whose ownership is difficult to understand.

## Reconstruct the component

Read the component and its relevant callers before proposing a structure.
Identify:

1. public entry points;
2. externally visible side effects;
3. the main success path;
4. failure and retry paths relevant to the task;
5. state that persists across calls or attempts; and
6. the last understood design when later changes obscured the original one.

Do not assume that everything currently located in the component belongs there.

## Classify fields and methods

Group each field and method by the responsibility it serves. For every group,
record:

| Question | Meaning |
| --- | --- |
| What state does it own? | Persistent, transient, or derived data |
| What invariant does it protect? | Conditions that must always hold |
| What starts and ends its lifecycle? | Creation, transition, and cleanup |
| What calls it? | Entry points and integration boundaries |
| What does it affect? | State mutation, RPC, files, or engine behavior |
| Why does it change? | The business or infrastructure reason |

A responsibility probably deserves its own component when it has an independent
state lifecycle, invariants, failure policy, vocabulary, test surface, or reason
to change.

## Test proposed ownership

For each proposed component, a reviewer should be able to complete this
sentence:

> This component owns ___, is called when ___, and may change ___.

If the sentence requires several unrelated clauses, the boundary is still too
broad. If the new component merely wraps arbitrary helpers, it is not a real
owner.

Keep managers focused on actual orchestration. Do not make them the default home
for every collection and transition needed by the operation they coordinate.

## Choose the smallest sufficient representation

State the concrete fact, event, or obligation before naming an abstraction.
Choose a representation for its actual contract, not for a hypothetical future
requirement:

- A boolean can express a yes/no fact. Model an unknown or unassigned state
  explicitly when the lifecycle requires it.
- Named modes clarify genuinely different algorithms or policies; they need
  not replace every boolean.
- A typed carrier can make a real asynchronous or integration boundary clear.
- A class is useful when behavior, invariants, or lifecycle have a cohesive owner.

For retained fields, returned values, options, and extension points, identify
their actual consumers. Derive stable values rather than storing another
authoritative copy. Keep snapshots, caches, and repeated state when different
lifetimes or consistency requirements justify them. When adapting metadata,
prefer carrying an existing record over redeclaring its schema, if the boundary
supports composition.

Before declaring something unused, trace positional as well as keyword calls,
inherited and dynamic dispatch, serialization, and external contracts relevant
to that API. Tests can observe fields that their owning class never reads.
An incomplete search is not proof of dead code. Verify the affected behavior
before removal, including empty inputs and failure paths when relevant.

Simplicity is fewer concepts and decisions for the maintainer, not fewer
classes or lines at any cost. Do not replace meaningful named values with
anonymous tuples, collapse distinct failure policies, or remove atomicity and
concurrency safeguards merely to shorten code. A proposed simplification must
preserve the invariant that justified the original representation.

## Audit names

First identify what a value or component represents to its callers. Name that
fact or responsibility, not merely its contents, implementation mechanism, or
consequences:

- State names identify the fact being recorded rather than a mechanism it affects.
- Component names identify the responsibility the component owns.
- Boundary-carrier names reveal the relevant communication or packaging role
  and message purpose that justify a separate type, rather than only naming
  the wrapped contents or an incidental lifecycle adjective.
- Operation names expose selection policy and observable side effects. A
  conditional eligibility check is not merely a historical lookup; reporting
  a failure to another component is not merely failing a local object.

Distinguish payload, metadata and transport in collection names. A queue of
payload/control events, records of acceptance progress, retry instructions and
buffered user rows are different facts, even when all concern the same batches.
Trace shared references and every writer before assigning a state label: a set
seeded with completed IDs may later also contain newly received IDs. Name its
actual membership, not only its initial contents or intended eventual state.
Test and diagnostic names must identify the exact observed boundary and identity;
storage acknowledgement, pipeline consumption and task completion are not
interchangeable, nor are logical slots and physical attempts.

Use these as decision criteria, not a mandatory naming template. A reviewer
should understand why the abstraction exists from its declaration and use site
without the implementation history. A naming defect does not by itself prove
the abstraction is unnecessary; evaluate its contract separately.

Avoid ambiguous words such as
`source`, `output`, `data`, `group`, or `replay` unless the surrounding contract
makes their exact meaning obvious.

In multi-stage systems, `producer`, `consumer`, and `source` are relative roles:
one stage can hold each role depending on the observer. Prefer the actual entity and
boundary, such as `upload_request_id`, `postgres_connection_factory`, or
`csv_import_chunks`. A reader should not have to infer the entity from
the directory, execution stage, or previous design discussion. Preserve actual
engine/protocol names and unambiguous technical uses such as graph sources;
this is not a blanket ban on those words.

Name a monkey-patch module against the external class, module, or function it
actually patches. Do not imply that an application-owned component needs
patching. Check the assignment target, including aliases, before choosing the
name; a purpose label alone can obscure which engine hook is being replaced.

At public boundaries, name the full contract, such as `import_partition_id`. Shorter
local names are acceptable only after the context is established. Avoid suffixes
that repeat information already obvious from local types and access patterns.

Agree with the user on newly introduced domain terminology when naming materially
affects how they will review and maintain the code.

When a concept is renamed, follow it through variables, parameters, internal
keys, callers, tests, and documentation; changing the class declaration alone
leaves the old mental model in place. Preserve established external contracts
unless their migration is authorized, and retain terminology where it still
describes a distinct, accurate concept.

## Warning signs

Pause before implementation when:

- a central class gains several unrelated collections or state machines;
- an integration patch becomes the owner of domain policy;
- callbacks cross several layers without a domain-level name;
- understanding a field requires searching the entire repository;
- persistent, transient, and derived state are mixed without distinction;
- a feature more than doubles an established sensitive module; or
- the proposed abstraction is easier to execute than to explain.
