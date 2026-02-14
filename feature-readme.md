# Store Locator Overview

This folder contains specifications, reference materials, and implementation documentation for the Store Locator Overview feature.

## Contents

| Path | Description |
|------|-------------|
| `handoff.md` | Full specification: user stories, acceptance criteria, state machine guidance, business logic, and implementation guidelines |
| `screenshots/` | UI states showing expected designs |
| `outputs/` | Generated documentation: state machines, gap report, implementation plan |

## Design Reference

### Screenshots (Design reference)

Static exports for contexts without Figma access.

| File | UI State | Handoff Section |
|------|----------|-----------------|
| `01-initial-state.png` | Empty search, Europe map view | 3.1.1 |
| `02-autocomplete.png` | Search input with suggestions dropdown | 3.1.2 |
| `03-results-relevance.png` | Results grouped by store type | 3.1.3 |
| `04-results-distance.png` | Results sorted by distance (flat list) | 3.1.4 |
| `05-no-results.png` | "No dealers found" state | 3.1.5 |
| `06-map-tooltip.png` | Map marker tooltip variations | 3.1.6 |

### Figma Links (Design reference)

For agents with the Figma MCP configured. These link directly to the design source.

| UI State | Desktop | Mobile |
|----------|---------|--------|
| Initial State | [View](figma-selection-link) | [View](figma-selection-link) |
| Autocomplete | [View](figma-selection-link) | [View](figma-selection-link) |
| Results (Relevance) | [View](figma-selection-link) | [View](figma-selection-link) |
| Results (Distance) | [View](figma-selection-link) | [View](figma-selection-link) |
| No Results | [View](figma-selection-link) | [View](figma-selection-link) |
| Map Tooltip | [View](figma-selection-link) | [View](figma-selection-link) |

> **Note:** Figma links require the Figma MCP to be configured. If unavailable, use the screenshots in the `screenshots/` folder.

## Scope

This documentation covers the **Store Locator Overview** only:

- Search functionality (location input, geolocation, autocomplete)
- Map display and interactions
- Store type legend and information overlay
- Search results list (grouping, sorting, store cards)
- Map tooltip interactions

**Out of scope:** Store Detail Page, Forms, map styling, Events, Booking Widget.

## Generated Outputs

The `outputs/` folder contains implementation documentation:

- `state-machines.md` — Mermaid diagrams for UI and data fetching states
- `gap-report.md` — Analysis of what is missing vs. the specification
- `implementation-plan.md` — Phased, TDD-driven implementation approach

Additional outputs may be generated during implementation:

- Data mapping strategies
- Phase completion summaries
- Testing strategies
- Architecture decisions
