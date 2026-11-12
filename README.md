# Agile Thinking Prompt

**A practical protocol for AI-assisted software delivery: small slices, explicit engineering decisions, and verifiable outcomes.**

Long-running AI-assisted projects become difficult to maintain when an assistant generates large changes before understanding the product, constraints, and existing code. This repository experiments with reusable instructions that bring product thinking, architecture discipline, and feedback loops into everyday implementation.

> **Status:** Experimental prompt toolkit. It is not a deployed agent, a Scrum implementation, or a benchmarked productivity tool. The templates are proposals to be evaluated against real projects.

## Start here

Use [Agile Delivery v2](model_instructions/agile-delivery-v2.md) as an AI coding-agent instruction, then provide a real issue and the relevant code/context. The agent should produce a **small, reviewable change**, not a full speculative rewrite.

1. Describe the user problem and what success looks like.
2. Provide repository context, constraints, and available validation commands.
3. Ask for one vertical slice with acceptance criteria and a short design rationale.
4. Review the implementation and test evidence before moving to the next slice.

**Expected output:** scope → acceptance criteria → decision/trade-off → implementation → verification → next slice.

See the [worked engineering example](docs/EXAMPLE_WORKFLOW.md), including a scoped backlog, an architecture decision, acceptance tests, and a release gate.

## Prompts

| File | Role |
| --- | --- |
| [Agile Delivery v2](model_instructions/agile-delivery-v2.md) | **Recommended:** outcome-driven execution with explicit evidence and a bounded work-in-progress limit |
| [Agile Thinking](model_instructions/agile-thinking.md) | Original experiment: planning-first development |
| [Code Craftsmanship](model_instructions/code-craftsmanship-protocol.md) | Original experiment: maintainable design and software craftsmanship |

The original prompts remain for comparison. V2 replaces mandatory private reasoning disclosures with a **concise public decision summary** and replaces ritualized “next sprints” with genuine, value-based next steps.

## Engineering principles

- **Product outcome over feature count.** Each change needs a user-visible or measurable outcome.
- **Architecture with trade-offs.** Prefer the smallest reversible design that preserves invariants and supports known requirements.
- **Continuous quality.** Acceptance criteria, regression tests, failure cases, and a Definition of Done are part of implementation, not an afterthought.
- **Transparency.** Distinguish inspected source code, executed checks, assumptions, and unverified claims.
- **Adaptive delivery.** Keep work in progress small; inspect results before expanding scope.

Inspired by iterative software development and the [official 2020 Scrum Guide](https://scrumguides.org/scrum-guide). A prompt is **not** a substitute for a self-managing Scrum Team, Product Owner, stakeholders, or actual inspection and adaptation.

## Evaluation before claiming effectiveness

Try the same scoped engineering task with and without V2 and compare: correct behavior, acceptance criteria coverage, change size, test evidence, regressions, and reviewer effort. **No comparative results are claimed yet.**

Suggestions and issues are welcome, especially concrete before/after examples and reproducible failures.

## Inspiration

The early experiment was inspired by [Thinking Claude](https://github.com/richards199999/Thinking-Claude). This repository explores a distinct emphasis: connecting user outcomes, design decisions, and verification to small deliverable increments.
