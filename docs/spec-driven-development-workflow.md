# Spec Driven Development

**AI-Assisted Software Development Workflow**

A comprehensive guide to structured, specification-first development with AI agents — from product requirements to production code.

*Version 1.0 | 24 March 2026*

---

## 1. Executive Summary

Spec Driven Development (SDD) is a structured workflow that combines human expertise with AI-agent capabilities to deliver software features with greater consistency, clarity, and quality. Rather than treating AI as a free-form code generator, SDD positions it as a disciplined collaborator — one that reads requirements, reasons about architecture, writes implementation plans, produces code, and validates its own work under clear human oversight at every stage.

The workflow integrates three key parties: Product Managers who articulate what needs to be built; Developers who provide technical direction, review, and approval; and AI Agents who accelerate the translation of intent into working software.

> **Why SDD works:** Traditional AI-assisted coding often results in misaligned implementations because AI has no shared understanding of intent. SDD solves this by creating a contract — the spec document — that aligns everyone before a single line of code is written.

---

## 2. The Problem SDD Solves

Without a structured workflow, AI-assisted development suffers from several compounding problems:

| Challenge | Impact on Delivery |
| :--- | :--- |
| Ambiguous requirements | AI interprets vague tickets in unexpected ways, producing code that does not match product intent. |
| No shared specification | Developers and AI work from different mental models, leading to rework cycles. |
| Absent implementation plan | AI jumps straight to code, skipping architectural reasoning and producing fragile solutions. |
| Untested code | AI-generated code without a testing step is harder to review and introduces regressions. |
| No structured review | Ad hoc code review of AI output misses patterns that a structured agent review would catch. |
| Lost knowledge | Without spec docs, knowledge lives in chat history and disappears between sessions. |

SDD addresses each of these by inserting structured artefacts — spec documents, implementation plans, and test suites — at the right points in the workflow, with human review gates between each phase.

---

## 3. Core Principles

### 3.1 Specification First

No implementation begins without an approved spec document. The spec is the single source of truth that aligns product managers, developers, and AI agents on what will be built, why, and how it fits into the existing system.

### 3.2 Human-in-the-Loop at Every Gate

SDD is not about replacing human judgement — it is about amplifying it. Humans set direction, review artefacts, and approve each transition between phases. AI executes within those boundaries.

### 3.3 Artefacts Over Chat

Every significant output — spec docs, implementation plans, code — is saved as a persistent artefact. Chat history is ephemeral; artefacts are durable and form a reviewable audit trail.

### 3.4 Tests as a Contract

Tests are not an afterthought. The AI agent is required to write tests as part of implementation, and those tests must pass before the implementation is considered complete. This creates a verifiable contract between intent and code.

### 3.5 Incremental Trust

The workflow is designed so that trust in AI output is earned incrementally. Small, reviewable artefacts build confidence before committing to larger ones. A spec is reviewed before a plan; a plan is reviewed before code is generated.

---

## 4. Roles and Responsibilities

| Role | Primary Responsibilities in SDD |
| :--- | :--- |
| Product Manager | Authors of the PRD. Creates Linear Issues with acceptance criteria. Reviews and approves spec documents. Confirms final implementation meets product intent. |
| Developer | Drives the AI agent through each workflow phase. Reviews spec documents, implementation plans, and generated code. Makes architectural decisions. Approves gate transitions. Raises pull requests and completes Linear Issues. |
| AI Agent (Spec) | Reads Linear Issues and generates comprehensive high-level spec documents covering requirements, architecture, data models, API contracts, and edge cases. |
| AI Agent (Plan) | Consumes approved spec documents and produces a detailed, step-by-step technical implementation plan with file-level context. |
| AI Agent (Implement) | Executes the approved implementation plan, writes code, writes tests, and verifies all tests passed. |
| AI Agent (Review) | Performs automated code review on pull requests, identifying bugs, style violations, security concerns, and deviations from spec. |

---

## 5. The SDD Workflow

The workflow is divided into ten steps across four phases: Requirements, Specification, Implementation, and Quality. Each phase concludes with a human review gate before proceeding.

---

### Phase 1 — Requirements

**Step 1 | Product Manager — Author the PRD & Create Linear Issues**

- Document the feature in a Product Requirements Document (PRD)
- Create corresponding Linear Issues with a clear, user-focused description
- Include measurable acceptance criteria — what does done look like?
- Link to relevant designs (Figma, mockups) and related issues
- Note any dependencies, priority, and estimates

---

### Phase 2 — Specification

**Step 2 | Developer — Brief the AI Agent on Linear Issues**

- Open an AI agent session and direct it to read the relevant Linear Issues
- Ask the agent to summarise its understanding before proceeding
- Correct any misunderstandings before moving to Step 3
- Provide additional codebase context the agent needs to read

**Step 3 | AI Agent — Generate the Spec Document**

- Feature overview and goals
- Scope — what is included, and explicitly what is not
- User stories and acceptance criteria
- System and data architecture changes
- API contracts (endpoints, request/response shapes)
- State management and data flow
- Error handling and edge cases
- Security and permission considerations
- Open questions that need resolution

**Step 4 | Product Manager + Developer — Review & Approve the Spec**

- Does the spec accurately reflect the original Linear Issue intent?
- Are all acceptance criteria addressed?
- Are edge cases and error states covered?
- Are data models and API contracts consistent?
- Use the Spec-Review Agent skill for a structured second opinion

> **GATE:** Spec must be explicitly approved before proceeding.

---

### Phase 3 — Implementation

**Step 5 | Developer + AI Agent — Create the Technical Implementation Plan**

- Ordered list of implementation tasks with file-level specificity
- New files to be created with their intended purpose
- Existing files to be modified and what changes are needed
- Database migrations or schema changes
- New dependencies to be introduced
- Testing strategy — unit, integration, and end-to-end tests

> **GATE:** Developer reviews and confirms the plan before execution begins.

**Step 6 | AI Agent — Execute the Implementation Plan**

- Work one task at a time; review diffs as they are produced
- Surface ambiguities rather than assuming
- Pause and update the plan if scope needs to change
- Commit logical units of work incrementally rather than one large commit

**Step 7 | AI Agent — Write Tests & Verify They Pass**

- Unit tests for all new functions and methods
- Integration tests for API endpoints and service boundaries
- Tests for each acceptance criterion from the spec
- Negative tests — error states, invalid inputs, permission failures
- All existing tests must continue to pass (no regressions)

> **GATE:** Zero failing tests before proceeding to quality phase.

---

### Phase 4 — Quality

**Step 8 | Developer + AI Agent — Polish the Code**

- Code Simplifier — reduces cyclomatic complexity and removes duplication
- Doc Generator — produces inline documentation and README updates
- Performance Advisor — flags N+1 queries and inefficient patterns
- Style Enforcer — applies project-specific style rules consistently

*Polishing is optional but recommended before code review.*

**Step 9 | Developer + AI Agent — AI-Assisted Code Review**

- Logic errors and potential runtime failures
- Security vulnerabilities — injection, authorisation, data exposure
- Deviation from the approved spec document
- Missing or weak test coverage
- Code style and project convention violations
- Performance concerns — unnecessary queries, blocking operations

*The developer resolves issues identified by the review before merge.*

**Step 10 | Developer — Complete the Linear Issue**

- Pull request merged to the main branch
- All automated CI checks passing
- Spec document linked to the Linear Issue
- Implementation notes or decisions documented
- Linear Issue status updated to Done
- Product manager notified for acceptance verification

---

## 6. Workflow Summary Diagram

| Step | Phase | Driver | Output | Gate? |
| :---: | :--- | :--- | :--- | :---: |
| 1 | Requirements | Product Manager | PRD + Linear Issues | |
| 2 | Specification | Developer | Briefed AI Agent | |
| 3 | Specification | AI Agent | Spec Document | |
| 4 | Specification | PM + Developer | Approved Spec | **GATE** |
| 5 | Implementation | Dev + AI Agent | Implementation Plan | **GATE** |
| 6 | Implementation | AI Agent | Working Code | |
| 7 | Implementation | AI Agent | Passing Test Suite | **GATE** |
| 8 | Quality | Dev + AI Agent | Polished Code | |
| 9 | Quality | Dev + AI Agent | Code Review Report | |
| 10 | Quality | Developer | Closed Linear Issue | **DONE** |

---

## 7. SDD Artefacts

SDD produces four primary artefacts. Each serves a distinct purpose and should be saved, linked to the Linear Issue, and retained for future reference.

### 7.1 Product Requirements Document (PRD)

Authored by the product manager. Describes the feature from a user and business perspective. This is the starting input to the SDD workflow and lives in the product team's documentation system.

### 7.2 Spec Document

Generated by the Spec Agent and reviewed by both the product manager and developer. This is the most critical artefact in SDD — it translates product intent into a technically precise blueprint. The spec document should be stored in the codebase repository (e.g., in a `docs/specs/` directory) alongside the code it describes.

### 7.3 Implementation Plan

Generated by the AI agent in Plan Mode from the approved spec. This is a developer-facing artefact that maps the spec to specific code changes. It is reviewed by the developer before execution and saved alongside the spec.

### 7.4 Test Suite

Written by the AI agent as part of implementation. The test suite is a living artefact — it documents expected behaviour and guards against future regressions. Tests are committed alongside the implementation code.

---

## 8. AI Agent Skills Used in SDD

| Skill | Purpose & Usage |
| :--- | :--- |
| Spec Agent | Used in Step 3. Reads Linear Issues and generates comprehensive spec documents. The primary knowledge-extraction and formalisation tool. |
| Spec-Review Agent | Used in Step 4. Reviews a generated spec document for gaps, inconsistencies, and missing edge cases. Assists human reviewers. |
| Plan Mode | Used in Step 5. Puts the AI agent into a structured planning mindset, producing a step-by-step implementation plan rather than jumping to code. |
| Code Simplifier | Used optionally in Step 8. Refactors working code to reduce complexity, remove duplication, and improve readability without changing behaviour. |
| Code Review Agent | Used in Step 9. Performs a structured review of pull request changes against the spec, checking for bugs, security issues, and coverage gaps. |

---

## 9. Human Review Gates

Review gates are the mechanism by which SDD ensures human oversight without becoming a bottleneck. There are four gates in the workflow:

### Spec Approval (after Step 4)

Both the product manager and the developer must explicitly approve the spec document. This is the most important gate — a bad spec will cascade problems through every subsequent step. Do not skip or rush this review.

### Plan Approval (after Step 5)

The developer reviews the implementation plan for technical correctness, completeness, and alignment with the spec. Edge cases in the plan should be resolved here, not during coding.

### Test Passage (after Step 7)

All tests written by the AI agent must pass before the quality phase begins. This is a non-negotiable automated gate — failing tests indicate incomplete or incorrect implementation.

### Pull Request Approval (after Step 9)

The developer reviews the AI code review report, addresses any issues, and obtains human approval of the pull request before merging. The AI review supplements but does not replace human code review.

---

## 10. Best Practices

### 10.1 Writing Good Linear Issues

- Always include acceptance criteria — the spec agent needs them to define done
- Be explicit about what is out of scope
- Link to relevant designs or related issues
- Include any known constraints (performance, accessibility, backwards compatibility)

### 10.2 Getting the Most from Spec Generation

- Brief the agent on relevant codebase context before running the Spec skill
- Ask the agent to confirm its understanding before generating the spec
- Review open questions in the spec and resolve them before plan creation
- Run the Spec-Review skill to catch gaps before human review begins

### 10.3 Effective Plan Review

- Check that every acceptance criterion from the spec appears in the plan
- Verify that the plan accounts for error states and edge cases
- Challenge any steps that seem to skip necessary complexity
- Ensure the testing strategy in the plan is proportionate to the risk of the change

### 10.4 Managing AI Agent Execution

- Review code incrementally as it is generated, not only at the end
- If the agent deviates from the plan, pause and re-anchor it to the approved plan
- Keep commits small and logically scoped for easier review and rollback
- Do not allow scope creep — out-of-scope changes need a new spec and plan

### 10.5 Code Review Quality

- Treat the AI code review as a first pass, not a final verdict
- Pay particular attention to security and permission-related findings
- Verify that the AI's review checked behaviour against the spec, not just style
- Always perform a human review in addition to the AI review

---

## 11. Common Pitfalls to Avoid

| Pitfall | How to Avoid It |
| :--- | :--- |
| Skipping the spec review gate | Establish a team norm that no plan may begin without both PM and developer sign-off on the spec. Make it visible on the Linear Issue. |
| Letting the AI improvise during implementation | If the agent encounters a gap in the plan, pause and update the plan rather than letting the agent make unilateral decisions. |
| Treating the spec as a formality | The spec is a working document. If it changes during review, update it before generating the plan. A stale spec produces a misaligned plan. |
| Ignoring test failures | Never mark a gate as passed when tests are failing. Investigate and fix root causes rather than suppressing failures. |
| Skipping the polish phase | AI-generated code is functional but often verbose or inconsistent. The polish phase pays dividends in maintainability. |
| Not linking artefacts to Linear | Spec docs and plans should be linked from the Linear Issue. Future developers need to understand why decisions were made. |
| Over-relying on AI code review | The AI code review skill is a tool, not a substitute for human judgement. It catches patterns; humans catch intent mismatches. |

---

## 12. Tooling Summary

| Tool / Platform | Role in SDD |
| :--- | :--- |
| Linear | Issue tracking and workflow state management. The source of truth for what is being built. |
| AI Agent (general) | The developer's intelligent assistant across all phases of the workflow. |
| Spec Agent Skill | Specialist skill for generating comprehensive spec documents from Linear Issues. |
| Spec-Review Agent Skill | Specialist skill for reviewing and improving spec documents before approval. |
| Plan Mode | AI agent configuration for structured implementation planning. |
| Code Simplifier Skill | Post-implementation code quality improvement. |
| Code Review Agent Skill | Automated pull request review against the spec and code quality standards. |
| Version Control (Git) | All artefacts and code are committed to version control. Spec docs live in the repo. |

---

## 13. Getting Started with SDD

1. **Start with one feature.** Choose a well-defined, medium-sized feature to pilot SDD. Avoid starting with a large, complex feature.
2. **Ensure Linear Issues are well-formed.** Review your team's Linear Issue template and add acceptance criteria as a required field.
3. **Configure your agent skills.** Set up the Spec Agent, Spec-Review Agent, and Code Review Agent skills in your AI agent environment.
4. **Run the workflow end-to-end.** Follow all ten steps in sequence, including four review gates. Do not skip steps on the first pilot.
5. **Retrospect after the pilot.** Gather feedback from the product manager and developer. Identify what worked, what was unclear, and what to refine.
6. **Standardise artefact storage.** Agree on where spec docs and plans are stored (e.g., `docs/specs/`) and how they are linked to Linear Issues.
7. **Scale to the team.** Once the pilot is validated, document your team's SDD conventions and roll out to all developers.

---

## 14. Glossary

| Term | Definition |
| :--- | :--- |
| SDD | Spec Driven Development — the structured AI-assisted development workflow described in this document. |
| PRD | Product Requirements Document — the product manager's description of what is to be built and why. |
| Spec Document | The AI-generated, human-reviewed specification that precisely defines what will be implemented. |
| Implementation Plan | The AI-generated, developer-approved step-by-step technical plan for implementing the spec. |
| Agent Skill | A specialised, pre-configured AI agent workflow designed for a specific task within SDD. |
| Plan Mode | An AI agent operating mode that prioritises producing structured plans over writing code. |
| Review Gate | A checkpoint in the SDD workflow that requires explicit human approval before proceeding. |
| Linear Issue | A work item in Linear that tracks a feature, bug, or task through the development lifecycle. |
| Artefact | A persistent, saved output from a workflow step (spec doc, implementation plan, test suite). |

---

*Spec Driven Development transforms AI from an unpredictable tool into a disciplined collaborator — one that amplifies human judgement rather than replacing it.*
