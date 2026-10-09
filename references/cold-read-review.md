# Cold-Read Review

Use a cold-read review before handing sensitive or cross-cutting work to the
human reviewer. Its purpose is to detect code that depends on conversation
history, hidden assumptions, or author-only context.

## Preserve independence

Give the cold reviewer only:

- the user-visible task or approved problem contract;
- governing repository instructions;
- the diff or isolated commit under review; and
- the minimum source needed to trace affected behavior.

Do not provide the intended explanation, suspected problems, design discussion,
or desired verdict. Use an isolated worktree or read-only snapshot when the
active working tree contains later changes.

## Ask the reviewer to reconstruct, not summarize

The reviewer must determine:

1. what each changed component owns;
2. what starts and ends each important state lifecycle;
3. the main success path;
4. relevant failure, retry, and cleanup paths;
5. why each new abstraction and data structure exists;
6. which behavior moved and which behavior changed;
7. which names hide a caller-relevant fact or responsibility, require absent
   context, retain an old concept at use sites, or leave the entity/boundary
   dependent on a context-specific abstract role instead of an identifiable
   entity, fact, or owned responsibility;
8. whether collection names distinguish content from descriptive records/control events,
   state labels remain true after every writer, and operation/test names expose
   their actual policy, side effect or observed completion boundary;
9. which decisions appear unsupported, ambiguous, or misplaced;
10. whether suspicious abstractions have a simpler equivalent, or which concrete
   invariant requires their complexity; and
11. what repeated collection operations mean in domain terms, their cumulative
   cost as state grows, and how retained incremental state is updated and reset.

The reviewer should cite concrete files and paths through the code. A generic
style assessment is not useful.
If these facts require an author explanation, record a clarity defect rather
than accepting a renamed helper or comment as proof that the code is clear.

## Use a strict finding threshold

Report only findings that affect correctness, workload cost, ownership, traceability,
debuggability, reviewability, or safe future modification. Separate:

- **Blocker:** the behavior or ownership cannot be determined safely.
- **Decision needed:** several materially different designs remain possible.
- **Clarity defect:** the code works, but a future engineer is likely to
  misunderstand or misuse it.
- **Verified:** an important boundary was reconstructed successfully.

Do not send raw reviewer narration to the user. The author must resolve clear
defects, return genuine decisions to the user, and include only unresolved
blockers plus concise verified evidence in the review handoff.

## Evaluate the skill itself

A successful review should expose real ambiguity when present and reconstruct
clear code without inventing issues. If it produces only generic checklist
language, revise the skill to demand more concrete evidence. If it requires the
design conversation to understand the code, treat that dependency as a product
finding rather than giving the reviewer more context.
