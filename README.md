# Spec-Driven Development: Four Approaches in Detailed Comparison

> **GitHub Spec Kit · BMAD Method · Amazon Kiro · AIUP (AI Unified Process)**
>
> As of: February 2026

---

## 1. Profiles

### 1.1 GitHub Spec Kit

- **Creator:** GitHub (Open Source, MIT)
- **Type:** CLI + Templates + Slash Commands
- **Stars:** ~69k (Feb 2026)
- **Initial release:** September 2024
- **Philosophy:** Agent-agnostic toolkit for Spec-Driven Development. Specs as version-controlled Markdown artifacts that any AI coding agent can consume.
- **Target audience:** Individual developers and teams working with any AI coding tool
- **IDE/tool binding:** None — works with Copilot, Claude Code, Gemini CLI, Cursor, Windsurf, etc.
- **Setup effort:** Low (`npx specify init`)

### 1.2 BMAD Method

- **Creator:** Adam Blackington / BMad Code LLC (Open Source)
- **Type:** Multi-agent framework with workflows, templates, and agent definitions
- **Stars:** ~35k (Feb 2026)
- **Current version:** v6
- **Philosophy:** A virtual Agile team of specialized AI agents covering the entire software lifecycle — from ideation through architecture to QA.
- **Target audience:** Teams and ambitious solo developers tackling complex projects in a structured way
- **IDE/tool binding:** None — works with Claude Code, Cursor, Gemini CLI, Copilot, etc.
- **Setup effort:** Medium (`npx bmad-method install`, module selection, configuration)

### 1.3 Amazon Kiro

- **Creator:** AWS (commercial product, GA since Nov 2025)
- **Type:** AI-powered IDE (VS Code fork)
- **Philosophy:** Specs are first-class artifacts within the IDE. From vibe coding to structured development through seamless switching between Spec and Prompt mode.
- **Target audience:** Developers and teams who prefer an integrated IDE experience
- **IDE/tool binding:** Kiro IDE (VS Code fork) — vendor lock-in to AWS/Kiro
- **Setup effort:** Low (download the IDE)

### 1.4 AIUP (AI Unified Process)

- **Creator:** Simon Martinelli / Martinelli LLC (Switzerland)
- **Type:** Methodology + Claude Code Plugins
- **Philosophy:** Requirements-centric development inspired by the Rational Unified Process (RUP). Specification is the source of truth, code is generated from it. AI is a consistency engine, not a creative driver.
- **Target audience:** Teams building long-lived business applications with stakeholder involvement
- **IDE/tool binding:** Claude Code (plugin system)
- **Setup effort:** Low (plugin installation in Claude Code)

---

## 2. Workflow Comparison

### 2.1 Spec Kit: Linear with Human Checkpoints

```
Constitution ─► /specify ─► /clarify ─► /plan ─► /tasks ─► /implement
     │              │            │          │          │          │
     │         [Human Review] [Q&A]  [Human Review] [Review]  [Review]
     ▼
  Project
  principles
```

**Phases in detail:**

1. **Constitution** (`/speckit.constitution`): Non-negotiable project principles — tech stack constraints, coding standards, testing philosophy. Created once and referenced by all subsequent phases.

2. **Specify** (`/speckit.specify`): Feature description in natural language as input. AI generates a structured `spec.md` with user stories, functional requirements, and acceptance criteria. Deliberately technology-free — no frameworks, no architecture.

3. **Clarify** (`/speckit.clarify`): Structured clarification round. AI identifies gaps, ambiguities, and edge cases in the spec. Asks targeted questions before planning begins.

4. **Plan** (`/speckit.plan`): This is where technology enters. Developer specifies stack and constraints. AI generates `plan.md` with architecture decisions, data model, API contracts, and test strategy. Additionally optional: `data-model.md`, `contracts/api-spec.json`, `research.md`.

5. **Tasks** (`/speckit.tasks`): AI breaks down the plan into an ordered, dependency-based task list. Tasks are grouped by user stories, with file paths and parallelization markers.

6. **Implement** (`/speckit.implement`): AI works through the tasks. Order: Contracts → Tests → Implementation (test-first).

**Iteration model:** Phases can be repeated (update spec → regenerate plan → update tasks). Everything version-controlled.


### 2.2 BMAD: Two-Phase Model with Agent Specialization

```
Phase 1: Agentic Planning (Web UI or IDE)

  Analyst ─► PM Agent ─► [UX Agent] ─► Architect ─► PO (Validation)
     │           │            │             │              │
  Brief       PRD        UX Spec      Architecture    Approval
                                      + DB Schema

Phase 2: Context-Engineered Development (IDE)

  SM Agent ──► Dev Agent ──► QA Agent ──► PR Review
     │              │             │            │
  Story Files    Code +        Tests     Multi-Agent
  (hyper-       Tests                     Review
   detailed)
```

**Phase 1 — Agentic Planning:**

1. **Analyst Agent ("Mary"):** Brainstorming with structured techniques (Role Playing, Five Whys, Analogical Thinking). Output: Project Brief with goals, constraints, risks.

2. **PM Agent ("John"):** Creates PRD from the brief. Contains Functional Requirements (with MoSCoW), Non-Functional Requirements, Epics, User Stories with Gherkin-style acceptance criteria. The PM prioritizes and defines MVP scope.

3. **UX Agent** (optional): Creates frontend specification and UI prompts when a user interface is needed.

4. **Architect Agent ("Winston"):** Creates `ARCHITECTURE.md` (or sharded into individual files) based on the PRD. Tech stack confirmation, database design (SQL DDL), API design, security concept.

5. **Product Owner:** Validates all artifacts for consistency and completeness. Quality gate before development begins.

**Phase 2 — Context-Engineered Development:**

6. **Scrum Master Agent:** Breaks epics into "hyper-detailed" story files. Each story contains all context (architecture references, business logic, test requirements) so the dev agent can work autonomously.

7. **Dev Agent:** Implements story by story in dedicated branches. Reads story file + relevant architecture sections.

8. **QA Agent:** Automated checks, code review, regression tests.

9. **PR Review:** Multi-agent review with automatic quality gates.

**Iteration model:** Story-based. SM creates next story → Dev implements → QA checks → next story. For architecture changes: back to the Architect Agent.

**Scaling adaptation:** BMAD automatically recognizes project complexity and offers three tracks (Quick Flow for small changes, Standard Flow, Enterprise Flow).


### 2.3 Kiro: Three-Part Spec Flow in the IDE

```
                    ┌──────────────┐
Prompt/Idea ──────► │ Spec Mode    │
                    │              │
Vibe Coding ──────► │  or          │
                    │              │
                    │ Vibe Mode    │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │ requirements │ ◄── EARS notation
                    │    .md       │     (WHEN/THEN/SHALL)
                    └──────┬───────┘
                           │ [Human Review + Refine]
                    ┌──────▼───────┐
                    │  design.md   │ ◄── Architecture, sequence
                    │              │     diagrams, interfaces
                    └──────┬───────┘
                           │ [Human Review + Refine]
                    ┌──────▼───────┐
                    │  tasks.md    │ ◄── Checkbox list with
                    │              │     max. 2 hierarchy levels
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │ Task Runner  │ ◄── IDE shows status,
                    │ (in IDE)     │     "Run all tasks"
                    └──────────────┘
```

**Workflow in detail:**

1. **Create spec:** User describes feature or bug in natural language. Kiro offers two starting points: "Requirements-First" or "Design-First". For bugfixes, there is a dedicated bugfix spec type.

2. **Requirements Phase:** Kiro generates `requirements.md` with user stories and EARS acceptance criteria — without asking sequential questions first (generate-then-iterate). User refines through dialogue.

3. **Design Phase:** After requirements are approved, Kiro analyzes the existing codebase and generates `design.md` with architecture, components, data model, sequence diagrams, and error handling. Kiro can independently conduct research during this phase.

4. **Task Phase:** From requirements + design, `tasks.md` is generated — a checkbox list with a maximum of two hierarchy levels. Tasks are strictly limited to coding activities (no deployment, no user tests).

5. **Execution:** Tasks can be run individually or collectively. IDE shows progress in real-time.

**Special features:**

- **Steering Files:** Global project documentation under `.kiro/steering/` (product.md, structure.md, tech.md). Kiro generates these initially through codebase analysis.
- **Hooks:** Event-driven automation (on file save → update tests, on commit → security scan).
- **Vibe ↔ Spec switching:** Users can freely switch between Vibe mode (unstructured) and Spec mode. Also: "vibe first, then generate spec."
- **Bugfix Specs:** Dedicated workflow with Current/Expected/Unchanged Behavior and regression protection through `SHALL CONTINUE TO`.

**Iteration model:** Spec files live in the repo (`.kiro/specs/`). Can be refined at any time. No fixed iteration structure — the user decides.


### 2.4 AIUP: Four RUP Phases with Stakeholder Validation

```
┌─────────────────────────────────────────────────────────────────┐
│                    Iterations across all phases                  │
│                                                                 │
│  Inception ──► Elaboration ──► Construction ──► Transition      │
│      │              │               │               │           │
│  Requirements   Use Case        AI generates    User Acceptance │
│  Catalog +      Diagrams +      Code + Tests    Testing +       │
│  Stakeholder    Entity Model +  from System     Deployment +    │
│  Alignment      System UC       Use Cases       Feedback        │
│                 Specs           [Dev Review]                     │
│                                                                 │
│  ◄─── Business validates ALL artifacts, not just business ────► │
└─────────────────────────────────────────────────────────────────┘
```

**Phases in detail:**

1. **Inception:**
   - Identify goals and stakeholders
   - Capture initial requirements in the catalog
   - Plan test strategy
   - Short iterations with rapid feedback

2. **Elaboration:**
   - Business Use Case Diagrams (PlantUML) — how the organization achieves goals
   - Entity Models (Mermaid ER diagrams with attribute tables)
   - System Use Case Diagrams — how the system supports goals
   - Business validates not only business artifacts but also entity models and use cases (!)
   - Derive test cases from use cases

3. **Construction:**
   - Detailed System Use Case Specifications (main flow, alternative flows, business rules)
   - AI (Claude Code) generates application code from specs
   - AI generates tests from use case flows
   - Developer reviews and corrects
   - On changes: update use case → regenerate code → tests protect behavior

4. **Transition:**
   - User Acceptance Testing
   - Continuous Delivery
   - Stakeholder feedback flows back into the next iteration
   - Production optimization

**Special features:**

- **Two use case levels:** Business Use Cases (domain understanding, not for code generation) and System Use Cases (behavioral contract, from which AI generates).
- **Stakeholders validate everything:** Not just business artifacts, but also entity models and system use cases. This catches domain modeling errors before they become expensive.
- **AI as consistency engine:** When requirements change, AI automatically updates all downstream artifacts (diagrams, specs, code, tests).
- **Diagrams-as-Code:** PlantUML for use cases, Mermaid for ER diagrams. Everything in Git, diffable, reviewable.
- **No task concept:** Use cases ARE the work units. There is no separate task breakdown — a system use case is directly translated into code.
- **Anti-determinism:** AIUP rejects the idea that perfect specs must produce deterministic output. Instead: iterative improvement. Tests protect behavior while code evolves.

**Iteration model:** True agile iterations. All phases run in short cycles. Specs, code, and tests improve together. Not waterfall.

---

## 3. Spec Formats in Detail

### 3.1 Requirements Notation

#### Spec Kit: User Stories + Free-Form Text

```markdown
### US-001: Task Board Management
**As a** project manager,
**I want** to move tasks between columns via drag-and-drop,
**So that** I can update task status without opening each task.

#### Acceptance Criteria
- [ ] User can drag a task card from one column to another
- [ ] Task status updates automatically after drop
- [ ] Other team members see the change in real-time
- [ ] Undo is available for 10 seconds after moving
```

**Strengths:** Easy to read, quick to write, no learning curve required.
**Weaknesses:** Acceptance criteria are free-form text — quality depends on the author. No formal structure for edge cases or error scenarios.

#### BMAD: Functional Requirements + Gherkin

```markdown
## Functional Requirements
- FR-001: [MUST] Task Board Drag-and-Drop
  The system must support drag-and-drop of tasks between
  Kanban columns.

  **Acceptance Criteria:**
  - Given a task in "To Do"
    When the user drags it to "In Progress"
    Then the task status changes to "In Progress"
    And all connected clients see the update within 2 seconds

  - Given a task is moved
    When the user clicks "Undo" within 10 seconds
    Then the task returns to its previous column

- FR-002: [SHOULD] Bulk Task Movement
  ...

## Non-Functional Requirements
- NFR-001: Performance — Drag operations < 100ms response time
- NFR-002: Concurrency — Support 50 simultaneous board users
```

**Strengths:** Gherkin (Given/When/Then) is an established standard. MoSCoW prioritization immediately shows what is Must/Should/Could. NFRs are explicit.
**Weaknesses:** Can become very verbose. Gherkin requires discipline. No natural workflow descriptions.

#### Kiro: EARS Notation

```markdown
### Requirement 1: Task Board Drag-and-Drop
**User Story:** As a project manager, I want to move tasks
between columns, so that I can update status efficiently.

#### Acceptance Criteria
1. WHEN a user drags a task card to a different column
   THEN the system SHALL update the task status to match
   the target column
2. WHEN a task status is updated
   THEN the system SHALL notify all connected clients
   within 2 seconds
3. WHEN a user moves a task AND clicks "Undo" within 10 seconds
   THEN the system SHALL restore the task to its previous column
4. IF the target column has a WIP limit reached
   THEN the system SHALL display a warning and prevent the move
5. WHILE a drag operation is in progress
   THE SYSTEM SHALL display a visual placeholder in the target column
```

**Strengths:** Formally testable through keywords (WHEN/THEN/SHALL, IF/THEN/SHALL, WHILE/SHALL). Different types of conditions clearly distinguished (event-driven, precondition-driven, state-driven). Well suited for automated test generation.
**Weaknesses:** Requires learning EARS syntax. Can be less intuitive for stakeholders than natural language. No concept for coherent workflows.

#### AIUP: System Use Case Specification

```markdown
# UC-003: Move Task

## Actor
Project Manager

## Preconditions
- Project Manager is logged into the board
- At least one task exists in the board
- Board has at least two columns

## Postconditions (Success)
- Task is located in the target column
- Task status matches the target column
- All connected clients display the current state

## Main Flow
1. Project Manager selects a task via drag
2. System displays visual placeholder in the target column
3. Project Manager drops the task in the target column
4. System checks WIP limit of the target column
5. System updates task status
6. System notifies all connected clients (< 2 sec)
7. System shows undo option for 10 seconds

## Alternative Flows

### 4a. WIP limit reached
4a.1. System displays warning "Column X has reached its limit"
4a.2. System prevents the drop
4a.3. Task returns to the source column

### 6a. Client unreachable
6a.1. System queues the notification
6a.2. On next connection: client synchronizes

## Business Rules
- BR-012: WIP limit per column configurable (default: 10)
- BR-013: Undo time window: 10 seconds, not configurable
- BR-014: Only tasks in own project can be moved
```

**Strengths:** Flows read like a story — accessible even for domain experts. Alternative flows systematically cover edge cases. Business rules are explicit. Pre- and postconditions clarify system boundaries. Directly translatable into code and tests.
**Weaknesses:** More effort to write. Can be overhead for simple features. Requires experience with use case modeling.

### 3.2 Technical Specs / Design

| Aspect | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **File** | `plan.md` | `ARCHITECTURE.md` (or `/docs/architecture/*.md`) | `design.md` | Entity Model + System UC → Code |
| **Scope** | Per feature | Per project (system-wide) | Per feature | Per project (system-wide) |
| **Tech stack** | ✅ explicit | ✅ explicit + rationale | ✅ explicit | ✅ in steering/config |
| **Data model** | `data-model.md` | `data-models.md` + `db-schema.md` (SQL DDL) | In `design.md` | Mermaid ER + attribute tables |
| **API design** | `contracts/api-spec.json` | In Architecture | In `design.md` | Derived from System UC |
| **Sequence diagrams** | ❌ | ❌ (not standard) | ✅ Mermaid | ✅ PlantUML |
| **Components** | In `plan.md` | `components.md` + `source-tree.md` | In `design.md` | Implicit through UC structure |
| **Security** | In `plan.md` | `security.md` (separate file) | In `design.md` | In architecture context |
| **Error handling** | In `plan.md` | `error-handling-strategy.md` | In `design.md` | In UC alternative flows |
| **Deployment** | ❌ | `infrastructure-and-deployment.md` | ❌ | ❌ (not in scope) |
| **Coding standards** | In `constitution.md` | `coding-standards.md` | In steering files | In project setup |

### 3.3 Task/Story Formats

#### Spec Kit: `tasks.md`

```markdown
## User Story 1: Task Board Setup
### T001 [P] - Create Task model
- File: `src/models/task.ts`
- Define Task interface with id, title, status, assignee
- Depends on: none

### T002 [P] - Create Board service
- File: `src/services/board.ts`
- Implement CRUD operations for tasks
- Depends on: T001

### T003 - Create Board API endpoint
- File: `src/routes/board.ts`
- REST endpoints: GET/POST/PUT/DELETE
- Depends on: T002
```

Characteristics: Flat list, explicit dependencies, `[P]` for parallelizable tasks, file paths directly specified.

#### BMAD: Story Files

```markdown
# Story 2.3: Implement Drag-and-Drop Task Movement

## Context
This story implements the core task movement functionality
defined in FR-001 of the PRD. It builds on the Board Service
(Story 2.1) and the WebSocket infrastructure (Story 2.2).

## Architecture Reference
- See: docs/architecture/components.md § Board Module
- Pattern: Event Sourcing for task state changes
- DB: PostgreSQL with advisory locks for concurrency

## Implementation Details
1. Create `DragDropHandler` component in `src/components/board/`
2. Use `@dnd-kit/core` for drag-and-drop (per architecture decision)
3. Emit `TaskMoved` event via WebSocket on successful drop
4. Implement optimistic UI update with rollback on failure

## Acceptance Criteria
- [ ] Given task in "To Do", When dragged to "In Progress",
      Then status updates and all clients notified within 2s
- [ ] Given WIP limit reached, When task dragged to column,
      Then warning shown and move prevented

## Dev Notes
- WebSocket handler is already in `src/ws/boardHandler.ts`
- Use existing `TaskRepository.updateStatus()` method
- Advisory lock key: `board_{boardId}_column_{columnId}`

## Testing Requirements
- Unit: DragDropHandler state management
- Integration: WebSocket broadcast on task move
- E2E: Full drag-and-drop cycle with two browser sessions
```

Characteristics: Contains ALL context. Dev agent needs no follow-up questions. References to architecture, PRD, and previous stories. Concrete file paths and methods.

#### Kiro: `tasks.md`

```markdown
- [x] 1. Set up drag-and-drop infrastructure
  - [x] 1.1 Install and configure @dnd-kit/core
  - [x] 1.2 Create DragContext provider component
  - [x] 1.3 Write unit tests for drag context
- [ ] 2. Implement task card dragging
  - [ ] 2.1 Add draggable behavior to TaskCard component
  - [ ] 2.2 Create drop zone for each column
  - [ ] 2.3 Write tests for drag-and-drop interaction
- [ ] 3. Add real-time sync
  - [ ] 3.1 Emit status change event on successful drop
  - [ ] 3.2 Subscribe to task movement events
  - [ ] 3.3 Write integration test with mock WebSocket
```

Characteristics: Checkbox list with a maximum of two levels. IDE shows progress visually. Only coding tasks (no deployment, no user tests). Can be executed automatically with "Run all tasks."

#### AIUP: No Task Concept

AIUP has no separate task breakdown. A system use case is directly translated into code:

```
/5_use_case_spec UC-003  →  Spec written
/6_implement UC-003      →  Code generated from spec
/7_karibu_test UC-003    →  Unit tests generated
/8_playwright_test UC-003 → Integration tests generated
```

Use cases ARE the work units. Each UC forms an independent, testable implementation step.

---

## 4. Architecture Documentation

| | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **Dedicated architecture document** | ❌ No | ✅ Yes, comprehensive | ❌ No (per feature) | ⚠️ Partial |
| **System-wide view** | `constitution.md` (principles) | `docs/architecture/` (15+ files) | `.kiro/steering/` (product, structure, tech) | Entity Model + UC Diagrams |
| **Where do architecture decisions land?** | In `plan.md` per feature | In Architecture + separate ADR structure possible | In `design.md` per feature | Implicit in System UCs |
| **Comparison with arc42** | Covers ~10% | Covers ~70% | Covers ~20% | Covers ~40% (different structure) |

### BMAD Architecture Structure (most detailed)

```
docs/architecture/
├── index.md                        # Table of contents
├── introduction.md                 # ≈ arc42 §1 Introduction
├── high-level-architecture.md      # ≈ arc42 §4 Solution Strategy
├── tech-stack.md                   # ≈ arc42 §4 (Technology decisions)
├── data-models.md                  # ≈ arc42 §5 Building Block View (Data)
├── components.md                   # ≈ arc42 §5 Building Block View
├── external-apis.md                # ≈ arc42 §3 Context & Scope
├── core-workflows.md               # ≈ arc42 §6 Runtime View
├── source-tree.md                  # Project structure
├── infrastructure-and-deployment.md # ≈ arc42 §7 Deployment View
├── error-handling-strategy.md      # ≈ arc42 §8 Cross-cutting Concepts
├── coding-standards.md             # ≈ arc42 §8 Cross-cutting Concepts
├── test-strategy-and-standards.md  # ≈ arc42 §10 Quality Requirements
├── security.md                     # ≈ arc42 §8 Cross-cutting Concepts
├── checklist-results-report.md     # Validation results
└── next-steps.md                   # Roadmap
```

### Kiro Steering (project-wide context documents)

```
.kiro/steering/
├── product.md     # Product vision, target audience, core features
├── structure.md   # Codebase architecture (auto-generated)
└── tech.md        # Tech stack, patterns, conventions
```

### AIUP: Architecture Through Models

AIUP has no explicit architecture document but expresses architecture through models:

- **Entity Model** (Mermaid ER): Data structure of the system
- **Use Case Diagrams** (PlantUML): System boundaries and actors
- **System Use Case Specs**: Behavior at system boundaries

The technical architecture (frameworks, deployment) is controlled through the plugin system (e.g., `aiup-vaadin-jooq` presupposes Vaadin + Spring Boot + jOOQ).

---

## 5. Traceability

| From → To | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| Business Goal → Requirement | ❌ implicit | ✅ Brief → PRD | ❌ implicit | ✅ Business UC → Req |
| Requirement → Design | ✅ spec → plan | ✅ PRD → Architecture | ✅ req → design | ✅ Req → System UC |
| Requirement → Task | ✅ spec → tasks (by story) | ✅ PRD → Story Files (FR IDs) | ✅ req → tasks (numbers) | N/A (UC = task) |
| Requirement → Code | ⚠️ implicit | ✅ Story → Branch | ⚠️ implicit via tasks | ✅ UC → generated code |
| Requirement → Test | ✅ AC → Tests | ✅ AC → Tests | ✅ EARS → Tests | ✅ UC Flows → Tests |
| Design → Code | ⚠️ implicit | ✅ Arch refs in stories | ⚠️ implicit via tasks | ✅ Entity Model → DB Schema |
| End-to-end | ⚠️ weak | ✅ good | ⚠️ medium | ✅ strong |

AIUP has the strongest end-to-end traceability because each system use case directly leads to code, tests, and database migrations. BMAD achieves good traceability through FR IDs that flow through all artifacts. With Spec Kit and Kiro, the connection between artifacts is convention-based rather than structurally enforced.

---

## 6. Stakeholder Involvement

| Aspect | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **Primary spec audience** | Developer + AI | AI agents (PM, Architect, Dev) | Developer + AI | Stakeholder + Developer + AI |
| **Business review built in?** | ❌ not in workflow | ⚠️ PO validates, but focus is on AI agents | ❌ not in workflow | ✅ core principle |
| **Can stakeholders read specs?** | ✅ User stories are readable | ⚠️ PRD is readable, Arch is not | ⚠️ EARS requires learning | ✅ UC flows are naturally readable |
| **Domain experts involved?** | ❌ | ❌ | ❌ | ✅ validate Entity Models + UCs |
| **Feedback loop** | Update spec, regenerate | PO validation gate | Refine spec in dialogue | Business validates in every iteration |

AIUP is the only approach that treats stakeholder involvement as a core principle — not an optional step. Martinelli's argument: In long-lived business systems, misalignment between stakeholders and implementation is more expensive than slow code generation.

---

## 7. AI Usage and Role Distribution

### Role of AI

| | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **AI generates specs** | ✅ from natural language | ✅ through specialized agents | ✅ from prompt/conversation | ⚠️ AI assists, human leads |
| **AI generates design** | ✅ plan.md | ✅ Architecture | ✅ design.md | ⚠️ Entity Model + UC partially |
| **AI generates code** | ✅ via /implement | ✅ via Dev Agent | ✅ via Task Runner | ✅ via /implement |
| **AI generates tests** | ✅ from AC | ✅ from Story AC | ✅ from EARS | ✅ from UC Flows |
| **AI reviews** | ✅ /analyze | ✅ QA Agent + PO Checklist | ⚠️ Hooks (on save/commit) | ❌ human reviews |
| **AI orchestrates** | ❌ | ✅ Orchestrator Agent | ⚠️ IDE controls flow | ❌ human controls |

### Human vs. AI — Who Decides What?

| Decision | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **What to build** | Human (input) | Human → Analyst | Human (input) | Human + Stakeholders |
| **Why** | Human (input) | Human → PM | Human (input) | Human + Stakeholders |
| **How (architecture)** | AI (plan.md) + Human (review) | AI (Architect) + Human (review) | AI (design.md) + Human (review) | Human decides, AI documents |
| **Implementation** | AI (implement) + Human (review) | AI (Dev Agent) + QA review | AI (Task Runner) + Human (review) | AI (implement) + Human (review) |
| **Quality assurance** | AI (/analyze) | AI (QA Agent) | AI (Hooks) | Human + AI tests |

AIUP gives humans the most control. BMAD delegates the most to AI agents. Spec Kit and Kiro fall in between.

---

## 8. Practical Considerations

### 8.1 Learning Curve

| | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **Entry barrier** | ⭐ Low | ⭐⭐⭐ High | ⭐⭐ Medium | ⭐⭐ Medium |
| **Productive after** | ~30 min | ~1-2 days | ~1-2 hours | ~1 day |
| **Concepts to learn** | 3 files + 5 commands | 12+ agents, workflows, sharding | 3 files + EARS + hooks | RUP phases, use cases, entity models |
| **Documentation** | README + spec-driven.md | Comprehensive docs site | Kiro Docs + tutorials | aiup.dev + blog + talks |

### 8.2 Overhead per Feature

| | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **Small bugfix** | Too much overhead | Too much overhead (Quick Flow helps) | Bugfix spec (appropriate) | Too much overhead |
| **Small feature (1-3 story points)** | ✅ Appropriate | ⚠️ Somewhat heavy | ✅ Appropriate | ⚠️ Somewhat heavy |
| **Medium feature (5-13 SP)** | ✅ Sweet spot | ✅ Sweet spot | ✅ Sweet spot | ✅ Sweet spot |
| **Complex system (greenfield)** | ⚠️ Lacks architecture | ✅ Sweet spot | ⚠️ Lacks system view | ✅ Sweet spot |
| **Enterprise / Compliance** | ⚠️ Too thin | ✅ Strong | ⚠️ Too thin | ✅ Strong |

### 8.3 Vendor Lock-in and Openness

| | Spec Kit | BMAD | Kiro | AIUP |
|---|---|---|---|---|
| **License** | MIT | Open Source | Proprietary (AWS) | Open Source (plugin) |
| **AI tool bound?** | ❌ Agent-agnostic | ❌ Agent-agnostic | ✅ Kiro IDE | ⚠️ Claude Code (plugin) |
| **Artifacts portable?** | ✅ Markdown in Git | ✅ Markdown/YAML in Git | ✅ Markdown in Git | ✅ Markdown + PlantUML in Git |
| **Usable without AI?** | ⚠️ Templates usable manually | ❌ Agents require AI | ❌ IDE requires AI | ⚠️ Methodology applicable manually |

---

## 9. Strengths and Weaknesses

### Spec Kit

| Strengths | Weaknesses |
|---|---|
| Minimal overhead, quickly productive | No system-wide architecture view |
| Agent-agnostic, no lock-in | Traceability only convention-based |
| Strict separation of spec (what) vs. plan (how) | No stakeholder involvement |
| Version-controlled artifacts | Too thin for complex systems |
| Constitution as governance instrument | No formal requirements notation |
| Active community (~69k stars) | Spec quality heavily depends on input |

### BMAD

| Strengths | Weaknesses |
|---|---|
| Most comprehensive architecture documentation | High learning curve (12+ agents) |
| Hyper-detailed story files = fewer follow-up questions | Can be overkill for small projects |
| Multi-agent QA and review | Token consumption from extensive contexts |
| Scales from bugfix to enterprise | Dependent on AI quality for agent orchestration |
| Brownfield workflows built in | Community-driven, no corporate backing |
| Expansion packs for domains (Gaming, DevOps) | PRD-centric, not stakeholder-centric |

### Kiro

| Strengths | Weaknesses |
|---|---|
| EARS makes requirements formally testable | Vendor lock-in to Kiro IDE |
| Bugfix spec with regression protection | EARS requires learning |
| Seamless vibe ↔ spec switching | No system-wide architecture |
| Hooks for automation | Per-feature view, no project view |
| Integrated IDE experience | AWS dependency |
| Codebase analysis for steering | Can be a sledgehammer for small bugs |

### AIUP

| Strengths | Weaknesses |
|---|---|
| Strongest stakeholder involvement | Currently only Claude Code plugin |
| Proven methodology (RUP-based) | Smaller community, less tooling |
| Clean problem/solution separation | Use case modeling requires experience |
| Best end-to-end traceability | Too heavy for prototypes/MVPs |
| Diagrams-as-Code (PlantUML, Mermaid) | Still stack-specific (Vaadin/jOOQ) |
| Iterative, not waterfall | No multi-agent orchestration |
| AI as consistency engine, human retains control | Requires RE competence in the team |

---

## 10. Decision Guide

### Choose Spec Kit if...
- you want to start quickly with minimal overhead
- you want to keep your preferred AI tool
- you are specifying a single feature or clearly scoped area
- you don't need formal architecture documentation
- you work as a solo developer or in a small team

### Choose BMAD if...
- you are starting a greenfield project with complex architecture
- you want maximum AI autonomy (multi-agent)
- you need comprehensive architecture documentation
- you have enterprise governance or compliance requirements
- you are willing to invest in the learning curve

### Choose Kiro if...
- you prefer an integrated IDE experience
- you want to transition from vibe coding to structured development
- you need formally testable requirements (EARS)
- you want to approach bugfixes systematically with regression protection
- you can live with the AWS lock-in

### Choose AIUP if...
- you are building long-lived business applications
- stakeholder alignment matters more than developer speed
- you come from the Java/Enterprise world (RUP experience helps)
- you need traceability from business requirement to line of code
- you are willing to invest in requirements engineering

### Combinations

These approaches are not mutually exclusive:

- **AIUP + Spec Kit:** AIUP for system-wide specification (use cases, entity model), Spec Kit for the feature implementation workflow
- **BMAD + Spec Kit:** BMAD for planning (PRD, architecture), Spec Kit templates for feature development
- **AIUP + arc42:** AIUP artifacts (entity model, UC diagrams) as input for arc42 documentation
- **Export Kiro Specs:** Use Kiro EARS requirements as input for BMAD or Spec Kit workflows
