# Agent Session Prompt Template

Use this prompt to initiate a gap analysis and implementation planning session with an AI agent.

---

## Prompt

```markdown
## Session: [Feature Name] - Gap Analysis & Implementation Planning

### Context

I am handing off a comprehensive specification document for the [Feature Name] 
feature. All reference materials and outputs for this work are organised in:

[path/to/docs/features/feature-name]/

This folder contains:

- `handoff.md` — Full specification with user stories, acceptance criteria, 
  state machine guidance, and implementation guidelines
- `screenshots/` — UI states showing expected designs
- `outputs/` — Where you will save your generated artifacts

Start by running `view [path/to/docs/features/feature-name]/` to familiarise 
yourself with the available materials. The `README.md` provides an overview 
and a screenshot reference table mapping filenames to UI states.

### Existing Implementation

The current implementation can be found in:

- [path/to/components]
- [path/to/styles]
- [path/to/tests]

Refer to CLAUDE.md (or equivalent project documentation) for project conventions, 
component architecture, and testing patterns.

### Your Tasks (In Order)

Work through these tasks sequentially. Each task builds on the previous ones, 
creating artifacts that collectively inform the final implementation plan. 
Pause after each task and share your output for my review before proceeding 
to the next.

---

**Task 1: Review & Orientation**

1. Read through the handoff document completely
2. Review all screenshots in the `screenshots/` folder
3. Explore the existing implementation to understand the current state
4. Note any initial questions or areas needing clarification

Share a brief summary of your understanding and any questions before proceeding.

---

**Task 2: Create State Machine Diagrams**

Based on Section 4 of the handoff document, create Mermaid state machine 
diagrams for:

1. **UI State Machine** — All user-facing states and transitions
2. **Data Fetching State Machine** — API/async states
3. **Parallel States** — Any independent state groups (e.g., tooltips, overlays)

Save as: `[path/to/docs/features/feature-name]/outputs/state-machines.md`

Include a brief explanation of each diagram and any assumptions you made.

**Pause for review before proceeding to Task 3.**

---

**Task 3: Gap Analysis**

Compare the specification (handoff.md + screenshots) and your state machine 
diagrams against the existing implementation.

Produce a structured gap report that includes:

1. **Summary** — High-level overview of implementation completeness
2. **Missing States** — UI or data states not implemented
3. **Missing Transitions** — State transitions not handled
4. **Incomplete Acceptance Criteria** — Reference by ID (e.g., AC-S01) with 
   details on what is missing
5. **Missing Tests** — Test coverage gaps
6. **Configuration Issues** — API setup, environment config, etc.

For each gap, assign a priority:
- **P0 (Critical):** Core functionality broken or missing
- **P1 (High):** Key user flow impacted
- **P2 (Medium):** Feature incomplete but workaround exists
- **P3 (Low):** Polish/enhancement

Save as: `[path/to/docs/features/feature-name]/outputs/gap-report.md`

**Pause for review before proceeding to Task 4. I will review the gap analysis 
and confirm which gaps are accurate. Once confirmed, the gap analysis becomes 
another artifact—alongside the handoff document, screenshots, and state 
machines—that informs the implementation plan.**

---

**Task 4: Implementation Plan**

Using the handoff document, screenshots, state machines, and the **confirmed 
gap analysis from Task 3**, propose a phased TDD implementation plan. The gap 
analysis identifies what work needs to be done; the other materials provide 
the context for how to do it. Do not proceed with this task until I have 
reviewed and signed off on the gap analysis.

Each phase should:
- Be focused and small enough for a single merge request
- Reference specific acceptance criteria being addressed (by ID)
- Include a test plan (unit, integration, visual regression as applicable)
- List implementation steps
- Note dependencies on previous phases
- Avoid disrupting development momentum—aim for incremental wins

Follow the phase template structure from Section 8 of the handoff document.

Save as: `[path/to/docs/features/feature-name]/outputs/implementation-plan.md`

---

### Working Approach

- **Sequential with checkpoints** — Complete each task fully before moving 
  to the next; wait for my feedback
- **Cumulative artifacts** — Each task produces an artifact that informs 
  subsequent tasks; the implementation plan draws on all of them
- **Gap analysis sign-off required** — Do not start Task 4 until I confirm 
  the gap analysis is accurate
- **Ask questions** — If the specification is ambiguous or you discover 
  contradictions, flag them
- **Follow project conventions** — Use existing patterns from the codebase
- **Be explicit about assumptions** — Document any assumptions in your outputs
- **Reference by ID** — When discussing acceptance criteria, always use the 
  AC-XXX identifiers

### Questions Before Starting?

Before beginning Task 1, do you have any questions about:
- The folder structure or file locations?
- The scope of this work?
- Any constraints or preferences I should clarify?
```

---

## Customisation Notes

1. **Replace all `[bracketed]` placeholders** with your actual paths and feature name

2. **Adjust task details** if your feature has specific requirements:
   - Add or remove state machine types based on complexity
   - Modify gap analysis categories to match your concerns
   - Adjust phase ordering suggestions in Task 4

3. **Add context about your stack** if relevant:
   - Testing frameworks in use
   - Component patterns to follow
   - API conventions

4. **Include Figma links** if you have MCP access:

   ```markdown
   - `screenshots/` — Static exports for offline reference
   - Figma links in README.md for live design access (requires Figma MCP)
   ```

---

## Example: Filled-In Prompt

See the `examples/` folder for a complete, filled-in version of this prompt
used for a Store Locator feature implementation.
