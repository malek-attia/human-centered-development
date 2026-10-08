# Human-Centered Development

An agent skill for code that people can understand, trace, debug, review, and
change—not just code that an agent can generate and execute.

## Why this skill exists

This skill grew out of a real collaboration problem: agent-written changes
worked, but reviewing them became a bottleneck. A human reviewer spent hours
tracing vague names, untangling responsibilities, discovering unstated design
decisions, and requesting rewrites. Sensitive modules kept accumulating state
and helper code, while tests were easier for an agent to run than for a teammate
to understand.

The bottleneck was not the reviewer's speed. It was the code's dependence on
the author's context. Once ownership, vocabulary, control flow, and review
boundaries became clear, review became much faster.

The skill makes that improvement part of the development cycle. It catches
unclear assumptions before implementation, discourages unnecessary abstractions,
and prevents confusing existing code from becoming the template for even more
confusing code.

> Code must be optimized for human comprehension, tracing, debugging, and
> contribution—not merely for automated execution.

This is not a formatting checklist or a rule to minimize line counts. A useful
class may simplify a real boundary; a shorter function may hide one. Correctness
and human comprehension both matter.

## How to use it

There are two layers: a standing project policy that makes the agent consider
the skill, and task-specific instructions loaded when the skill applies.

### 1. Install the skill and make the policy part of project instructions

Install this skill folder using your agent's documented skill-discovery mechanism.
Keep its name `human-centered-development` and include `SKILL.md` and
`references/`. `agents/openai.yaml` supplies optional agent-specific metadata;
the development instructions themselves do not depend on that file or a
particular model, language, framework, or repository.

**Installation alone is not enough.** In an early trial, an agent with the skill
installed barely called it when the standing project instruction was missing.
Automatic discovery makes a skill available; it does not guarantee frequent use.

Copy [DEVELOPMENT_POLICY.md](DEVELOPMENT_POLICY.md) into your project, for example
as `docs/development-policy.md`. Add the following to `AGENTS.md`, `CLAUDE.md`,
or the equivalent instruction file your agent automatically loads:

```markdown
## Human-centered development

Code must be optimized for human comprehension, tracing, debugging, and
contribution—not merely for automated execution.

Read and follow [the development policy](docs/development-policy.md) before
code work. Use the human-centered-development skill proactively for substantial
design, implementation, refactoring, and review tasks; do not wait for the user
to request it. Read its SKILL.md and applicable references before acting.
For trivial isolated edits, follow the policy without the full workflow.
If the skill is unavailable, report that and still follow the policy.
```

Adjust the policy path to its actual location. A Markdown link is not a promise
that an agent will load its target automatically: the instruction above explicitly
requires reading it. Alternatively, put the policy directly in the always-loaded
instruction file. Check your agent's instruction scope and loading rules.

This repository's [AGENTS.md](AGENTS.md) demonstrates the policy reference.

The compact policy should be present in the agent's working context. The full
skill and all five references should **not** be loaded for every task: the skill
routes to only the guidance needed for the current work.

After setup, try a fresh session with a realistic request such as “Extend this
existing coordinator with retry handling.” Check that the agent reads the policy
and relevant skill instructions, reconstructs ownership, and flags material
unsettled decisions before coding. If it does not, verify instruction loading and
skill discovery rather than assuming installation was sufficient.

If your agent has no skill-discovery mechanism, use the same policy and explicitly
give it the path to this package's `SKILL.md`. Do not claim automatic skill
activation in that setup.

### 2. Apply it to the right work

Use the skill when:

- Extending an existing module whose responsibilities or state have become
  difficult to understand.
- Designing sensitive infrastructure, concurrent state transitions, retries,
  recovery, cleanup, or integration boundaries.
- Writing adapters, callbacks, or monkey patches that might absorb policy
  belonging elsewhere.
- Separating a behavior-preserving refactor from a new feature so each can be
  reviewed independently.
- Reviewing agent-written code for hidden assumptions, misleading names, or
  abstractions whose purpose requires conversation history.
- Building or restructuring tests that a person must run, inspect, and debug.

Useful requests include:

- “Audit this module's responsibilities before adding the feature.”
- “Make this change easy to trace; separate refactoring from behavior changes.”
- “Review this diff without relying on our earlier design conversation.”
- “Make this failure scenario independently runnable and diagnostically clear.”

You can name the skill explicitly; agents that support `$skill-name` invocation
can use `$human-centered-development`. The standing policy is there so explicit
invocation is not required for every applicable task.

Skip the full workflow for trivial isolated edits. Scale review and validation
to the actual risk; this skill does not require a new architecture discussion
for every change or replace behavioral tests.

## What the skill does

[SKILL.md](SKILL.md) is the entry point. It routes work to:

- [Responsibility audit](references/responsibility-audit.md): reconstruct
  behavior, ownership, representations, and vocabulary.
- [Design-decision gates](references/design-decision-gates.md): distinguish
  local choices, evidence-dependent assumptions, and decisions needing approval.
- [Reviewable-change workflow](references/reviewable-change-workflow.md): keep
  existing behavior, new features, and unrelated work separate.
- [Cold-read review](references/cold-read-review.md): check whether an engineer
  can reconstruct the change without its author explaining it.
- [Human-readable tests](references/human-readable-tests.md): make scenarios,
  actions, expectations, commands, and failure evidence visible.

Project contracts and approved vocabulary still govern the work. The skill does
not authorize unrelated redesigns, commits, publication, or changes to user data.
Improve it using concrete review failures, not speculative rules for every
possible situation.
