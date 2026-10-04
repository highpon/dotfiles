---
name: review-gated-workflow
description: >-
  Enforces a disciplined review-gated workflow for software development where major decisions,
  implementation plans, architectural changes, and final diffs require explicit review checkpoints
  and user sign-off before proceeding. Use this skill when the user requests high-rigor development,
  safe refactoring, careful planning, or explicit review gates before applying changes.
---

# Review-Gated Workflow

The Review-Gated Workflow is a disciplined engineering practice designed to combine the velocity of agentic AI with human oversight, ensuring high code quality, architectural consistency, and safety.

## Core Principles

1. **AI Proposes, Human Approves**: Propose structured plans, architecture options, and diffs; await explicit confirmation at designated gates.
2. **Never Blind-Execute**: Do not jump into implementation without an agreed-upon plan.
3. **Evidence-Based Verification**: Every code modification must be verified by automated tests, type checks, or linting before moving to the next gate.
4. **Early Escalation**: If unexpected roadblocks or scope changes emerge, stop and re-align rather than silently improvising.

---

## The Four Review Gates

```
  [User Request]
        │
        ▼
┌─────────────────────────┐
│ 1. Plan & Scope Gate    │ ──(Approval)──┐
└─────────────────────────┘               │
        ▲ (Revision)                      ▼
        └─────────────────── ┌─────────────────────────┐
                             │ 2. Design/Arch Gate     │ (If new deps/schema)
                             └─────────────────────────┘
                                          │ (Approval)
                                          ▼
                             ┌─────────────────────────┐
                             │ 3. Implementation Gate  │ (TDD + Verification)
                             └─────────────────────────┘
                                          │
                                          ▼
                             ┌─────────────────────────┐
                             │ 4. Pre-Completion Gate  │ ──(Final Review)──► [Done]
                             └─────────────────────────┘
```

---

### Gate 1: Plan & Scope Gate (Before Writing Code)

Before writing or modifying any implementation code:

1. **Understand Intent & Requirements**:
   - Inspect existing code, documentation, and tests.
   - Clarify ambiguities rather than making silent assumptions.
2. **Draft Implementation Plan**:
   - **Objective**: Concise summary of what will be achieved.
   - **Proposed Changes**: Exact list of files to add, edit, or remove with brief rationale.
   - **Verification Strategy**: How the changes will be tested (unit tests, integration tests, commands).
   - **Risks & Edge Cases**: Potential failure modes or breaking changes.
3. **Gate Checkpoint**:
   - Present the plan clearly to the user.
   - **PAUSE** and wait for user approval or feedback before proceeding to write code.

---

### Gate 2: Architecture & Dependency Gate (When Applicable)

Triggered whenever a task introduces new external libraries, schema alterations, or significant pattern changes:

1. **Identify Alternatives**:
   - Outline at least two approaches (e.g., standard library vs third-party dependency, approach A vs approach B).
   - Highlight trade-offs (maintenance overhead, binary size, performance, security).
2. **Gate Checkpoint**:
   - Present the architectural decision record (ADR) or summary to the user.
   - Confirm library licenses and compatibility with project standards.
   - Wait for explicit user selection/approval.

---

### Gate 3: Incremental Implementation & Verification Gate

During code modification:

1. **Test-First / Verification-First**:
   - Write or update tests that reproduce the issue or assert new behavior.
   - Ensure tests fail initially (Red) before implementation if applicable.
2. **Atomic & Focused Commits**:
   - Keep changes scoped strictly to the agreed plan.
   - Do not perform unrelated refactoring in the same step.
3. **Mid-Flight Deviation Check**:
   - If an assumption is invalidated or unforeseen complexity arises:
     - **STOP immediately**.
     - Document the findings and present revised options to the user.
     - Do not continue implementation on invalidated premises.

---

### Gate 4: Pre-Completion & Merge Gate (Before Marking Done)

Before marking the task complete or committing:

1. **Run Automated Verification**:
   - Execute test suites, linters, and formatters (`make test`, `go test ./...`, `npm test`, etc.).
   - Verify that all checks pass cleanly with zero new warnings.
2. **Prepare Final Review Summary**:
   - **Diff Summary**: Bulleted list of modified files and key changes made.
   - **Verification Evidence**: Output from test runs confirming success.
   - **User Impact**: Any changes in configuration, behavior, or user-facing APIs.
   - **Residual Tasks / Next Steps**: Any follow-up items or non-blocking suggestions.
3. **Gate Checkpoint**:
   - Ask the user for final review and sign-off.
   - Only commit, push, or declare completion after explicit user approval.
