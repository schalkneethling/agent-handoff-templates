# Store Locator Overview - Implementation Handoff Document

## Document Purpose

This document provides specifications for the Store Locator Overview feature. It enables an AI Agent to:

1. **Create state machine diagrams** for UI states, data fetching, and user interactions
2. **Perform gap analysis** comparing this specification against the existing implementation
3. **Propose a phased, TDD-driven implementation plan** with focused merge requests

---

## 1. Feature Overview

### 1.1 Objective

The Store Locator enables visitors to find the nearest stores (own locations, partner locations, and dealers) where they can experience products in person.

### 1.2 Scope

This document covers **Store Locator Overview only**:

- Search functionality (location input, geolocation, autocomplete)
- Map display and interactions
- Store type legend and information overlay
- Search results list (grouping, sorting, store cards)
- Map tooltip interactions

**Out of scope:** Detail pages, forms, map styling, events, booking widgets.

### 1.3 Key Assumptions

- Map defaults to regional view (no IP-based detection on initial load)
- User location retrieved only after explicit permission grant
- Distance calculations use **road distance**, not straight-line

---

## 2. User Stories & Acceptance Criteria

### 2.1 Search

#### US-SEARCH-01: Location-Based Search

> **As a user**, I want to search for stores by entering a city or postal code, so that I can find nearby locations.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-S01 | Autocomplete suggestions | Location-based suggestions only (not store names) |
| AC-S02 | Search trigger | Selecting a suggestion triggers search |
| AC-S03 | Results display | Results based on location and defined search radius |
| AC-S04 | URL persistence | Search criteria added to URL (shareable) |
| AC-S05 | Manual search | Search button triggers search if no suggestion selected |

#### US-SEARCH-02: Current Location Search

> **As a user**, I want to use my current location to find nearby stores.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-L01 | Permission request | Button triggers browser location permission |
| AC-L02 | Location pin | User's location shown on map if permitted |
| AC-L03 | Results display | Results based on user's location |
| AC-L04 | Permission denied | Appropriate feedback provided |

---

### 2.2 Map

#### US-MAP-01: Store Location Visualisation

> **As a user**, I want a map displaying store locations based on my search.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-M01 | Default view | Map defaults to regional view |
| AC-M02 | View mode | Standard view (no satellite/terrain) |
| AC-M03 | Centre after search | Map centres on searched location |
| AC-M04 | Store markers | Type-specific icons for each store type |
| AC-M05 | Mobile scroll | Page scrolls to map after search on mobile |

**Map Interactions:**

| ID | Criterion | Details |
|----|-----------|---------|
| AC-M06 | Zoom controls | Zoom via buttons |
| AC-M07 | Pinch zoom | Zoom via trackpad gesture |
| AC-M08 | Shift-scroll | Shift + scroll enables zoom |
| AC-M09 | Pan | Click and drag to move |
| AC-M10 | List independence | Map panning/zooming does NOT affect results list |

---

### 2.3 Results List

#### US-RESULTS-01: Grouped & Sortable Results

> **As a user**, I want results grouped by store type and sortable by relevance or distance.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-R01 | Results count | Display count (e.g., "14 results in Rome, Italy") |
| AC-R02 | Sort dropdown | Options: "Relevance" (default), "Distance" |
| AC-R03 | Relevance sort | Grouped by store type |
| AC-R04 | Within-group sort | By ranking score; if equal, by distance |
| AC-R05 | Distance sort | Flat list, closest first |
| AC-R06 | Type headers | Section headers shown only for relevance sort |
| AC-R07 | Type info icon | Header icons open information overlay |
| AC-R08 | No results state | Empty state with illustration |

---

### 2.4 Store Card

#### US-CARD-01: Store Card Display

> **As a user**, I want to see essential details for each store and access the detail page.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-C01 | Card image | Store image (optional) |
| AC-C02 | Store name | Name displayed |
| AC-C03 | Store type icon | Icon indicating type |
| AC-C04 | Address | Full address |
| AC-C05 | Distance | Road distance from search location |
| AC-C06 | Details list | Certifications/features |
| AC-C07 | Store logo | Logo displayed |
| AC-C08 | CTA button | Button to detail page |

**Card Interactions:**

| ID | Criterion | Details |
|----|-----------|---------|
| AC-C10 | CTA click | Navigates to detail page |
| AC-C11 | Hover effect | Zoom effect on image |
| AC-C12 | Map highlight | Highlights corresponding map pin |

---

### 2.5 Map Tooltip

#### US-TOOLTIP-01: Map Marker Interaction

> **As a user**, I want to click map markers to see a preview card.

| ID | Criterion | Details |
|----|-----------|---------|
| AC-T01 | Marker click | Clicked marker enlarges |
| AC-T02 | Tooltip card | Shows image, name, type icon, address |
| AC-T03 | Card clickable | Opens store detail page |
| AC-T04 | Close button | X button closes tooltip |
| AC-T05 | Replace behaviour | Clicking another marker replaces tooltip |
| AC-T06 | Mobile support | Tap behaviour matches click |

---

## 3. UI States

### 3.1 State Inventory

| State | Description | Key Visual Indicator |
|-------|-------------|---------------------|
| Initial | Page loaded, no search | Empty input, regional map view |
| Autocomplete Active | User typing | Dropdown with suggestions |
| Results (Relevance) | Search complete | Grouped sections with headers |
| Results (Distance) | Search complete | Flat list sorted by distance |
| No Results | No stores found | Empty state illustration |
| Map Tooltip | Marker clicked | Enlarged marker with card |

---

## 4. State Machine Specification

### 4.1 UI State Machine

**States:**

- `INITIAL`
- `AUTOCOMPLETE_ACTIVE`
- `SEARCHING`
- `RESULTS_RELEVANCE`
- `RESULTS_DISTANCE`
- `NO_RESULTS`
- `ERROR`

**Key Transitions:**

| From | Event | To |
|------|-------|-----|
| INITIAL | input_focus + typing | AUTOCOMPLETE_ACTIVE |
| INITIAL | geolocation_click | SEARCHING |
| AUTOCOMPLETE_ACTIVE | suggestion_selected | SEARCHING |
| AUTOCOMPLETE_ACTIVE | escape_key | INITIAL |
| SEARCHING | results_received (count > 0) | RESULTS_RELEVANCE |
| SEARCHING | results_received (count = 0) | NO_RESULTS |
| SEARCHING | error | ERROR |
| RESULTS_RELEVANCE | sort_changed | RESULTS_DISTANCE |
| RESULTS_DISTANCE | sort_changed | RESULTS_RELEVANCE |
| RESULTS_* | new_search | SEARCHING |

**Parallel States:**

- Map Tooltip: `TOOLTIP_CLOSED` ↔ `TOOLTIP_OPEN`
- Information Overlay: `OVERLAY_CLOSED` ↔ `OVERLAY_OPEN`

### 4.2 Data Fetching State Machine

**States:**

- `IDLE`
- `FETCHING_AUTOCOMPLETE`
- `FETCHING_GEOLOCATION`
- `FETCHING_SEARCH_RESULTS`
- `CALCULATING_DISTANCES`
- `SUCCESS`
- `ERROR`

---

## 5. Business Logic

### 5.1 Search Radius

Search radius varies by country and store type. Example:

| Country | Dealers | Partners | Own Stores |
|---------|---------|----------|------------|
| Country A | 10 km | 30 km | 80 km |
| Country B | 50 km | 100 km | 100 km |
| Country C | 25 km | 50 km | 100 km |

Distance calculated using **road distance** from search location.

### 5.2 Ranking Logic

Stores sorted by `rankingScore` (pre-calculated in search index). Higher score = listed higher. If scores equal, closer distance wins.

### 5.3 Store Types

| Type | Icon Style | Description |
|------|------------|-------------|
| Own Store | Filled | Company-owned locations |
| Partner Store | Outline | Partner locations |
| Dealer | Grey outline | Dealer partners |

---

## 6. Technical Context

### 6.1 Technology Stack

| Component | Technology |
|-----------|------------|
| Search/Filtering | Algolia InstantSearch.js |
| Autocomplete | Algolia Autocomplete.js |
| Map | Leaflet + OpenStreetMap |
| Distance Calculation | Routing API (e.g., Geoapify, Google) |

### 6.2 URL Structure

URL parameters for shareability:

- Location/coordinates or search term
- Sort order (if not default)

---

## 7. Gap Analysis Instructions

### 7.1 Approach

1. **Review existing implementation** — state management, components, API integration
2. **Map to state machine** — verify each state and transition exists
3. **Verify acceptance criteria** — check implementation and test coverage
4. **Document gaps** — structured report with priorities

### 7.2 Priority Classification

| Priority | Definition |
|----------|------------|
| P0 - Critical | Core functionality broken or missing |
| P1 - High | Key user flow impacted |
| P2 - Medium | Feature incomplete, workaround exists |
| P3 - Low | Polish/enhancement |

---

## 8. Implementation Planning Guidelines

### 8.1 TDD Approach

1. Write failing tests describing expected behaviour
2. Implement minimum code to pass
3. Refactor while keeping tests green
4. Document decisions

### 8.2 Phase Structure

Each phase should be:

- **Focused:** Single responsibility
- **Small:** Frequent merge requests
- **Testable:** Clear acceptance criteria
- **Deployable:** No regressions

### 8.3 Suggested Phase Ordering

1. Foundation — State machine, URL handling
2. Search Input — Autocomplete, geolocation
3. Results Display — List rendering, sorting
4. Map Integration — Markers, centre/zoom
5. Card Interactions — Hover effects, highlighting
6. Tooltips — Marker click behaviour
7. Distance Calculation — Routing API
8. Edge Cases — Empty states, errors
9. Polish — Animations, accessibility

---

## Appendix: Acceptance Criteria Checklist

### Search

- [ ] AC-S01–S05: All search criteria

### Geolocation

- [ ] AC-L01–L04: All geolocation criteria

### Map

- [ ] AC-M01–M10: All map criteria

### Results

- [ ] AC-R01–R08: All results criteria

### Store Card

- [ ] AC-C01–C12: All card criteria

### Tooltip

- [ ] AC-T01–T06: All tooltip criteria

---

*Document version: 1.0*
*Status: Ready for gap analysis*
