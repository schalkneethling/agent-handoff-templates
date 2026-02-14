# [Feature Name] - Implementation Handoff Document

## Document Purpose

This document provides comprehensive specifications for the [Feature Name] feature. It is intended to enable an AI Agent to:

1. **Create state machine diagrams** representing UI states, data fetching states, and user interaction flows
2. **Perform gap analysis** by comparing these specifications against the existing implementation
3. **Propose a phased, TDD-driven implementation plan** with small, focused merge requests

---

## 1. Feature Overview

### 1.1 Objective

[Describe what this feature enables users to do and why it matters.]

### 1.2 Scope

This document covers **[Feature Name]** only:

- [List in-scope functionality]
- [List in-scope functionality]
- [List in-scope functionality]

**Explicitly out of scope:**

- [List out-of-scope items]
- [List out-of-scope items]

### 1.3 Key Assumptions

- [List assumptions about the implementation context]
- [List assumptions about external dependencies]
- [List assumptions about user behaviour]

---

## 2. User Stories & Acceptance Criteria

### 2.1 [User Story Category]

#### US-[ID]-01: [Story Title]

> **As a [user type]**, I want to [action], so that I can [benefit].

**Acceptance Criteria:**

| ID | Criterion | Details |
|----|-----------|---------|
| AC-[ID]-01 | [Criterion name] | [Detailed description] |
| AC-[ID]-02 | [Criterion name] | [Detailed description] |
| AC-[ID]-03 | [Criterion name] | [Detailed description] |

#### US-[ID]-02: [Story Title]

> **As a [user type]**, I want to [action], so that I can [benefit].

**Acceptance Criteria:**

| ID | Criterion | Details |
|----|-----------|---------|
| AC-[ID]-01 | [Criterion name] | [Detailed description] |
| AC-[ID]-02 | [Criterion name] | [Detailed description] |

---

## 3. UI States & Visual Reference

### 3.1 State Inventory

#### 3.1.1 [State Name]

**Description:** [When this state occurs]

**Visual characteristics:**

- [Element visibility/state]
- [Data displayed]
- [Available interactions]

#### 3.1.2 [State Name]

**Description:** [When this state occurs]

**Visual characteristics:**

- [Element visibility/state]
- [Data displayed]
- [Available interactions]

### 3.2 Visual Reference Summary

| State | Key Visual Indicator | Screenshot Reference |
|-------|---------------------|---------------------|
| [State Name] | [Distinguishing feature] | [Screenshot filename] |
| [State Name] | [Distinguishing feature] | [Screenshot filename] |

---

## 4. State Machine Specification

### 4.1 UI State Machine

#### States

- `INITIAL` — [Description]
- `[STATE_NAME]` — [Description]
- `[STATE_NAME]` — [Description]
- `ERROR` — [Description]

#### Transitions

| From State | Event | To State | Side Effects |
|------------|-------|----------|--------------|
| INITIAL | [event_name] | [STATE] | [Effects] |
| [STATE] | [event_name] | [STATE] | [Effects] |

### 4.2 Data Fetching State Machine

#### States

- `IDLE` — [Description]
- `FETCHING_[DATA]` — [Description]
- `SUCCESS` — [Description]
- `ERROR` — [Description]

#### Transitions

| From State | Event | To State | Side Effects |
|------------|-------|----------|--------------|
| IDLE | [event_name] | FETCHING_[DATA] | [Effects] |

### 4.3 Parallel States

[Document any independent state machines that run alongside the main UI state machine, such as tooltips, modals, or async processes.]

---

## 5. Business Logic & Data Requirements

### 5.1 [Business Rule Category]

[Document business rules, calculations, or logic that cannot be inferred from the UI.]

| Condition | Rule | Example |
|-----------|------|---------|
| [Condition] | [Rule description] | [Example] |

### 5.2 [Data Requirement Category]

[Document data structures, field requirements, or API response expectations.]

| Field | Type | Purpose | Required |
|-------|------|---------|----------|
| [field_name] | [type] | [purpose] | Yes/No |

### 5.3 External Dependencies

[Document any external APIs, services, or data sources the feature depends on.]

| Dependency | Purpose | Documentation |
|------------|---------|---------------|
| [Service name] | [What it provides] | [Link] |

---

## 6. Technical Context

### 6.1 Technology Stack

| Component | Technology | Notes |
|-----------|------------|-------|
| [Component] | [Technology] | [Relevant notes] |

### 6.2 Existing Implementation

[Describe what already exists, if anything, and where to find it.]

| Component | Location | Status |
|-----------|----------|--------|
| [Component] | [File path] | Complete/Partial/Missing |

### 6.3 Configuration Requirements

[Document any environment variables, feature flags, or configuration needed.]

| Configuration | Purpose | Status |
|---------------|---------|--------|
| [Config name] | [Purpose] | Exists/Missing |

---

## 7. Gap Analysis Instructions

When performing gap analysis, compare this specification against the existing implementation and produce a structured report:

1. **Summary** — High-level overview of implementation completeness
2. **Missing States** — UI or data states not implemented
3. **Missing Transitions** — State transitions not handled
4. **Incomplete Acceptance Criteria** — Reference by ID with details
5. **Missing Tests** — Test coverage gaps
6. **Configuration Issues** — Environment, API, or setup issues

**Priority Classification:**

- **P0 (Critical):** Core functionality broken or missing
- **P1 (High):** Key user flow impacted
- **P2 (Medium):** Feature incomplete but workaround exists
- **P3 (Low):** Polish/enhancement

---

## 8. Implementation Planning Guidelines

### 8.1 TDD Approach

1. Write failing tests describing expected behaviour
2. Implement minimum code to pass tests
3. Refactor while keeping tests green
4. Document decisions and deviations

### 8.2 Phase Structure

Each phase should be:

- **Focused:** Single responsibility or closely related features
- **Small:** Completable in a reasonable timeframe
- **Testable:** Clear acceptance criteria
- **Deployable:** No breaking changes

### 8.3 Suggested Phase Template

```markdown
## Phase [N]: [Phase Name]

**Acceptance Criteria Addressed:** AC-[ID]-01, AC-[ID]-02

**Dependencies:** Phase [N-1] (if applicable)

**Implementation Steps:**
1. [Step]
2. [Step]

**Test Plan:**
- Unit: [What to test]
- Integration: [What to test]
- Visual: [What to test]

**Estimated Effort:** [Small/Medium/Large]
```

---

## Appendix A: Acceptance Criteria Checklist

| ID | Criterion | Status |
|----|-----------|--------|
| AC-[ID]-01 | [Criterion] | ⬜ |
| AC-[ID]-02 | [Criterion] | ⬜ |

---

## Appendix B: Screenshot Reference

[Include descriptions of each screenshot for agents without direct image access.]

### [Screenshot filename]

[Detailed description of what the screenshot shows]

---

*Document ready for agent handoff.*
