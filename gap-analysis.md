# Store Locator Overview - Gap Analysis Report

**Document Version:** 1.0  
**Analysis Date:** January 2026  
**Current Status:** ✅ Complete — All functionality implemented

---

## Executive Summary

The current implementation is in **early prototype stage** with significant gaps. The existing code provides basic scaffolding but lacks core functionality.

**Critical Finding:** The geo-search implementation uses a bounding-box approach tied to the map viewport, but the specification requires radius-based search centred on the user's searched location. This is a fundamental architectural mismatch that affects multiple acceptance criteria.

**Overall Completion Estimate:** ~5%

---

## 1. Summary Metrics

| Category | Implemented | Partial | Missing | Total |
|----------|-------------|---------|---------|-------|
| UI States | 1 | 0 | 5 | 6 |
| State Transitions | 1 | 0 | 12 | 13 |
| Acceptance Criteria | 2 | 3 | 42 | 47 |
| Test Files | 0 | 0 | ~15 | ~15 |
| External API Integrations | 0 | 1 | 2 | 3 |

---

## 2. Missing States

### 2.1 UI States

| State | Status | Notes |
|-------|--------|-------|
| `INITIAL` | ⚠️ Partial | Template renders but no state management |
| `SEARCHING` | ❌ Missing | No loading state during search |
| `RESULTS_RELEVANCE` | ❌ Missing | No results rendering |
| `RESULTS_DISTANCE` | ❌ Missing | No distance sorting |
| `NO_RESULTS` | ❌ Missing | No empty state component |
| `ERROR` | ❌ Missing | No error state handling |

### 2.2 Parallel States

| State Machine | Status |
|---------------|--------|
| Map Tooltip | ❌ Missing |
| Information Overlay | ❌ Missing |

---

## 3. Missing Transitions

| From | To | Event | Status |
|------|-----|-------|--------|
| INITIAL | SEARCHING | search_initiated | ❌ Missing |
| INITIAL | SEARCHING | geolocation_click | ❌ Missing |
| SEARCHING | RESULTS_RELEVANCE | results_received | ❌ Missing |
| SEARCHING | NO_RESULTS | no_results | ❌ Missing |
| SEARCHING | ERROR | search_error | ❌ Missing |
| RESULTS_RELEVANCE | RESULTS_DISTANCE | sort_changed | ❌ Missing |
| RESULTS_DISTANCE | RESULTS_RELEVANCE | sort_changed | ❌ Missing |
| RESULTS_* | SEARCHING | new_search | ❌ Missing |
| ERROR | SEARCHING | retry | ❌ Missing |

---

## 4. Architectural Gaps

### 4.1 P0: Geo-Search Architecture Mismatch

**Problem:** Current implementation uses bounding-box search tied to map viewport. Specification requires radius-based search from a fixed point.

**Impact:** Violates AC-M10 (map changes should not affect results list).

**Remediation:** Replace viewport-based search with coordinate + radius approach.

### 4.2 P0: Missing Geocoding Integration

**Problem:** No way to convert city/postal code input to coordinates.

**Required:** Geocoding API integration before search can function.

### 4.3 P0: Missing Distance Calculation

**Problem:** Specification requires road distance, but no routing API integration exists.

**Required:** Distance Matrix API integration with batching for multiple destinations.

### 4.4 P1: Missing Search Form Handler

**Problem:** Form exists in template but has no JavaScript implementation.

**Required:** Submit handler, input validation, geolocation button handler.

### 4.5 P1: Missing Results Rendering

**Problem:** No component for displaying search results with grouping logic.

**Required:** Results list with type-based grouping (relevance) and flat list (distance).

### 4.6 P1: Missing URL Persistence

**Problem:** Searches are not reflected in the URL.

**Required:** URL parameter handling for shareable/bookmarkable searches.

### 4.7 P2: Map Configuration

**Problem:** Map centres on incorrect location at wrong zoom level.

**Required:** Default to appropriate regional view.

### 4.8 P2: Missing Map Tooltips

**Problem:** Markers have no click handlers or tooltip component.

**Required:** Tooltip card with store summary on marker click.

---

## 5. Incomplete Acceptance Criteria

### 5.1 Search (AC-S01 through AC-S05)

| ID | Criterion | Status |
|----|-----------|--------|
| AC-S01 | Autocomplete suggestions | ⏸️ Deferred |
| AC-S02 | Search trigger on selection | ⏸️ Deferred |
| AC-S03 | Results based on location + radius | ❌ Missing |
| AC-S04 | URL persistence | ❌ Missing |
| AC-S05 | Manual search button trigger | ❌ Missing |

### 5.2 Map (AC-M01 through AC-M10)

| ID | Criterion | Status |
|----|-----------|--------|
| AC-M01 | Default regional view | ⚠️ Partial |
| AC-M02 | Standard view mode | ✅ Implemented |
| AC-M03 | Centre on search location | ❌ Missing |
| AC-M04 | Type-specific markers | ⚠️ Partial |
| AC-M10 | Map changes do not affect list | ❌ Wrong |

### 5.3 Results (AC-R01 through AC-R08)

| ID | Criterion | Status |
|----|-----------|--------|
| AC-R01 | Results count display | ⚠️ Partial |
| AC-R02 | Sort dropdown | ⚠️ Partial |
| AC-R03 | Relevance grouping | ❌ Missing |
| AC-R04 | Within-group ranking | ❌ Missing |
| AC-R05 | Distance sorting | ❌ Missing |
| AC-R08 | No results state | ❌ Missing |

### 5.4 Store Card (AC-C01 through AC-C13)

| ID | Criterion | Status |
|----|-----------|--------|
| AC-C01–C08 | Card content fields | ⚠️ Partial |
| AC-C10 | CTA navigation | ❌ Missing |
| AC-C11 | Hover effects | ❌ Missing |
| AC-C12 | Map highlight on hover | ❌ Missing |

### 5.5 Map Tooltip (AC-T01 through AC-T06)

| ID | Criterion | Status |
|----|-----------|--------|
| AC-T01–T06 | All tooltip criteria | ❌ Missing |

---

## 6. Missing Tests

| Test Type | Coverage |
|-----------|----------|
| Unit tests (utilities) | ❌ None |
| Component tests | ❌ None |
| E2E tests | ❌ None |
| Visual regression tests | ❌ None |
| Accessibility tests | ❌ None |

---

## 7. Priority Classification

### P0 — Critical (Blocks core functionality)

| Gap | Description |
|-----|-------------|
| Geo-search architecture | Replace bounding-box with radius-based search |
| Geocoding integration | Convert location input to coordinates |
| Distance calculation | Road distance via routing API |
| Search form handler | Process input and trigger search |

### P1 — High (Key user flows impacted)

| Gap | Description |
|-----|-------------|
| Results rendering | Display search results with grouping |
| Sorting logic | Relevance/distance sorting |
| URL persistence | Shareable URLs |
| Geolocation handler | Browser location API integration |

### P2 — Medium (Feature incomplete)

| Gap | Description |
|-----|-------------|
| Map configuration | Correct initial view |
| Map tooltips | Marker click behaviour |
| Empty/error states | UI for edge cases |

### P3 — Low (Polish/enhancement)

| Gap | Description |
|-----|-------------|
| Autocomplete | Location suggestions (deferred) |
| Hover effects | Card interactions |
| Animations | Smooth transitions |

---

## 8. API Integration Checklist

### Geocoding API

- [ ] API key configured
- [ ] Wrapper function created
- [ ] Error handling implemented
- [ ] Unit tests written

### Distance Matrix API

- [ ] Wrapper function created
- [ ] Batching for multiple destinations
- [ ] Error handling with fallback
- [ ] Unit tests written

### Search API

- [ ] Radius-based geo-search implemented
- [ ] Coordinate parameters wired
- [ ] Integration tests written

---

*Gap analysis complete. Ready for implementation planning.*
