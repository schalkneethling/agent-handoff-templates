# Agent Handoff Templates

Templates and examples for preparing feature specifications that enable AI agents to perform gap analysis and propose implementation plans.

## Overview

This repository accompanies the blog series [Preparing Feature Specifications for AI Agents](https://schalkneethling.com/blog/structuring-feature-specifications-for-ai-agents), which covers a practical workflow for handing off implementation work to AI coding agents like Claude Code or Cursor.

The approach treats AI agents as collaborators that need structured context—not magic boxes that need instructions. The templates here provide that structure.

## Contents

```text
agent-handoff-templates/
├── README.md
├── templates/
│   ├── handoff-template.md        # Blank template for your own features
│   ├── session-prompt-template.md # Prompt to kick off an agent session
│   └── folder-structure.md        # Recommended folder organisation
└── examples/
    ├── feature-readme.md          # Example README for a feature folder
    ├── handoff.md                 # Complete example specification
    ├── state-machines.md          # Example agent output: state diagrams
    └── gap-analysis.md            # Example agent output: gap report
```

### Templates

**`handoff-template.md`** — A blank template with all the sections needed for a comprehensive feature specification. Includes placeholders for user stories, acceptance criteria, UI states, state machine guidance, business logic, and technical context.

**`session-prompt-template.md`** — The prompt structure for initiating an agent session. Defines sequential tasks with checkpoints, specifies output locations, and sets expectations for collaboration.

**`folder-structure.md`** — Recommended folder organisation for keeping specifications, screenshots, and agent outputs together in your repository.

### Examples

**`feature-readme.md`** — Example README for a feature folder. Shows how to create an orientation document that maps screenshots to handoff sections, includes Figma links for agents with MCP access, and lists generated outputs.

**`handoff.md`** — A complete example specification for a Store Locator feature. Demonstrates how to structure scope definitions, traceable acceptance criteria (47 criteria across 6 user stories), UI state inventories, and business logic documentation.

**`state-machines.md`** — Example output from an agent session. Shows Mermaid state machine diagrams for UI states, data fetching, tooltips, and parallel state management. Includes implementation notes reflecting how the actual build simplified the original design.

**`gap-analysis.md`** — Example output from an agent session. Demonstrates a structured gap report with priority classifications (P0–P3), acceptance criteria traceability, missing state/transition analysis, and test coverage gaps.

## The Workflow

1. **Create a handoff document** — Distil your PRD, designs, and requirements into a focused specification with traceable acceptance criteria.

2. **Organise reference materials** — Set up a folder with the handoff document, screenshots, and an outputs directory for agent artifacts.

3. **Run the agent session** — Use the session prompt to guide the agent through sequential tasks: orientation, state machines, gap analysis, implementation plan.

4. **Review at checkpoints** — Pause between tasks to verify understanding and refine outputs before the agent builds on them.

5. **Implement from the plan** — Use the phased implementation plan as your roadmap, with each phase sized for a single merge request.

## Blog Series

This repository accompanies a three-part blog series:

1. **[Structuring Feature Specifications for AI Agents](https://schalkneethling.com/blog/structuring-feature-specifications-for-ai-agents)** — How to structure a handoff document with traceable acceptance criteria, UI state inventories, and explicit business logic.

2. **[Organising Reference Materials for AI Agent Sessions](https://schalkneethling.com/blog/organising-reference-materials-for-ai-agents)** — How to set up a self-contained folder structure with screenshots, Figma links, and output directories.

3. **[Running an AI Agent Implementation Planning Session](https://schalkneethling.com/blog/running-an-ai-agent-implementation-session)** — How to structure the session prompt with sequential tasks, checkpoints, and the complete prompt template.

## Usage

1. Copy the templates to your project's documentation folder
2. Fill in the handoff template with your feature's requirements
3. Add screenshots or Figma links for visual reference
4. Use the session prompt to start work with your AI agent
5. Review outputs at each checkpoint before proceeding

## Adapting for Your Project

The examples use a Store Locator feature, but the structure applies to any feature with enough complexity to warrant formal specification:

- Dashboard components with multiple states
- Form wizards with validation logic
- Search interfaces with filtering and sorting
- Interactive maps or data visualisations
- Multi-step workflows with error handling

The key is having clear acceptance criteria, defined UI states, and explicit business logic—regardless of the domain.

## Contributing

If you have improvements to the templates or additional examples that would help others, contributions are welcome. Please open an issue to discuss significant changes before submitting a pull request.

## License

MIT License. See [LICENSE](LICENSE) for details.

---

Created by [Schalk Neethling](https://schalkneethling.com). Built while working on real client projects, refined through practical use.
