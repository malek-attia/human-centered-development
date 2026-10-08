# Reviewable Change Workflow

Use this workflow when a requested feature exposes structural problems in
existing code or when old and new behavior are already mixed in the working tree.

## Establish the boundary

Explicitly classify each intended edit as one of:

- pre-existing behavior moved without contract changes;
- new behavior required by the task;
- necessary integration between the two; or
- unrelated work that must remain untouched.

Do not call an edit a refactor when it changes external behavior, state
transitions, retry semantics, ordering, or failure handling.

## Refactor the existing behavior first

When practical:

1. Reconstruct the refactor from the last committed behavior.
2. Extract cohesive responsibilities without importing feature behavior.
3. Keep public contracts and observable behavior stable.
4. Validate through the real integration surface, not only imports.
5. Present it as an independent review boundary.
6. Commit it only when the user authorized a commit.
7. Leave the new behavior as a separate staged, unstaged, or later committed
   change according to the requested workflow.

If old and new work are mixed locally, an isolated Git worktree can help
reconstruct and validate the old-behavior refactor without disturbing ongoing
feature work. Use it only when that separation actually helps. Before bringing
the refactor back, inspect the original working changes and choose an integration
method that preserves them. Verify that the remaining feature diff still contains
all new behavior and excludes the refactor.

Never overwrite or discard the user's working changes to manufacture this
separation.

## Keep the diff readable

Avoid unrelated formatting, parameter folding or unfolding, file-wide renaming,
or movement of unaffected code. Preserve the original layout unless a specific
structural change requires otherwise.

Minimal diffs are not an excuse to keep an incorrect ownership boundary. Make
the necessary refactor, but isolate it and explain the reason.

## Keep boundaries thin

An adapter, monkey patch, callback, or protocol boundary should normally:

1. intercept or receive the event;
2. obtain engine- or protocol-owned information;
3. translate it into explicit project-domain arguments;
4. call the component that owns the operation; and
5. preserve the original external behavior.

Move domain state machines, retry policy, bookkeeping, and diagnostics into
named owners that can be understood independently.

## Validate and hand off

Validate both sides of the separation:

- the refactor still satisfies established behavior;
- the feature tests exercise only the new behavior they claim to cover; and
- no feature-only state or API leaked into the refactor commit.

For each affected module, report:

- what changed;
- what responsibility it owns;
- why that responsibility belongs there;
- the important control flow and side effects;
- how it maps to the task; and
- the exact validation performed.
