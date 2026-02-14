# Recommended Folder Structure

This document describes the recommended folder organisation for feature specifications and agent outputs.

---

## Structure

```text
docs/features/[feature-name]/
├── README.md                    # Orientation for agents and developers
├── handoff.md                   # Full specification document
├── screenshots/                 # Visual reference (static exports)
│   ├── 01-[state-name].png
│   ├── 02-[state-name].png
│   └── ...
└── outputs/                     # Agent-generated artifacts
    ├── .gitkeep                 # Ensures folder is tracked before outputs exist
    ├── state-machines.md        # Generated in Task 2
    ├── gap-report.md            # Generated in Task 3
    └── implementation-plan.md   # Generated in Task 4
```

---

## File Descriptions

### README.md

The entry point for both agents and human developers. Should include:

```markdown
# [Feature Name]

This folder contains specifications, reference materials, and implementation 
documentation for the [Feature Name] feature.

## Contents

| Path | Description |
|------|-------------|
| `handoff.md` | Full specification: user stories, acceptance criteria, state machine guidance |
| `screenshots/` | UI states showing expected designs |
| `outputs/` | Generated documentation: state machines, gap report, implementation plan |

## Screenshot Reference

| File | UI State | Handoff Section |
|------|----------|-----------------|
| `01-initial-state.png` | [Description] | 3.1.1 |
| `02-[state-name].png` | [Description] | 3.1.2 |

## Figma Links (Optional)

If using Figma MCP, include direct links to design frames:

| UI State | Desktop | Mobile |
|----------|---------|--------|
| Initial State | [View](figma-link) | [View](figma-link) |

## Scope

**In scope:**
- [Feature area]
- [Feature area]

**Out of scope:**
- [Related feature not covered]

## Generated Outputs

After the implementation planning session, the `outputs/` folder will contain:

- `state-machines.md` — Mermaid diagrams for UI and data fetching states
- `gap-report.md` — Analysis of what is missing vs. the specification
- `implementation-plan.md` — Phased, TDD-driven implementation approach
```

### handoff.md

The complete feature specification. See `templates/handoff-template.md` for the full structure.

### screenshots/

Static exports of UI designs, numbered to match the handoff document sections:

- **Naming convention:** `[number]-[state-name].png`
- **Examples:** `01-initial-state.png`, `02-autocomplete-active.png`
- **Purpose:** Visual reference for agents without Figma MCP access

### outputs/

Directory for agent-generated artifacts:

- **state-machines.md** — Mermaid state diagrams created in Task 2
- **gap-report.md** — Gap analysis created in Task 3
- **implementation-plan.md** — Phased implementation plan created in Task 4

The `.gitkeep` file ensures the directory is tracked in git before any outputs exist.

---

## Placement in Your Repository

### Option A: Dedicated features folder (recommended)

```text
your-repo/
├── docs/
│   ├── backend/
│   ├── frontend/
│   ├── features/              ← Feature specifications go here
│   │   ├── store-locator/
│   │   ├── checkout-flow/
│   │   └── user-dashboard/
│   └── testing/
└── src/
```

**Benefits:**

- Clear separation between "how we build" (other docs) and "what we build" (features)
- Easy to find all feature documentation in one place
- Natural home for agent-generated artifacts

### Option B: Co-located with source

```text
your-repo/
├── src/
│   ├── components/
│   │   └── store-locator/
│   │       ├── docs/          ← Specification lives with the code
│   │       │   ├── handoff.md
│   │       │   ├── screenshots/
│   │       │   └── outputs/
│   │       ├── StoreLocator.tsx
│   │       └── ...
│   └── ...
└── ...
```

**Benefits:**

- Documentation stays close to the code it describes
- Easier to keep in sync during changes
- Natural for monorepo component libraries

---

## Version Control Considerations

### What to commit

| Content | Commit? | Notes |
|---------|---------|-------|
| `README.md` | ✅ Yes | Documentation |
| `handoff.md` | ✅ Yes | Specification |
| `screenshots/` | ⚠️ Consider | Can be large; provides versioned baseline |
| `outputs/*.md` | ✅ Yes | Valuable documentation of decisions |

---

## Creating the Structure

### Quick setup script

```bash
#!/bin/bash
# create-feature-docs.sh

FEATURE_NAME=$1

if [ -z "$FEATURE_NAME" ]; then
  echo "Usage: ./create-feature-docs.sh feature-name"
  exit 1
fi

mkdir -p "docs/features/$FEATURE_NAME/screenshots"
mkdir -p "docs/features/$FEATURE_NAME/outputs"
touch "docs/features/$FEATURE_NAME/outputs/.gitkeep"
touch "docs/features/$FEATURE_NAME/README.md"
touch "docs/features/$FEATURE_NAME/handoff.md"

echo "Created docs/features/$FEATURE_NAME/"
echo "Next steps:"
echo "  1. Copy handoff-template.md content to handoff.md"
echo "  2. Add screenshots to screenshots/"
echo "  3. Fill in README.md"
```

### Usage

```bash
chmod +x create-feature-docs.sh
./create-feature-docs.sh store-locator
```

---

## Multiple Features

As you add more features, your structure grows naturally:

```text
docs/features/
├── store-locator/
│   ├── README.md
│   ├── handoff.md
│   ├── screenshots/
│   └── outputs/
├── checkout-flow/
│   ├── README.md
│   ├── handoff.md
│   ├── screenshots/
│   └── outputs/
└── user-dashboard/
    ├── README.md
    ├── handoff.md
    ├── screenshots/
    └── outputs/
```

Each feature is self-contained—an agent can work on one without needing context from others.
