# Store Locator Overview - State Machine Diagrams

> **Note**: This document reflects the actual implementation, which uses a simplified architecture compared to the original specification.

**Last Updated**: January 2026  
**Status**: ✅ Synchronised with implementation

---

## What Changed from Original Design

The implementation uses a **simplified state machine** compared to the original specification:

| Aspect | Original Design | Actual Implementation |
|--------|-----------------|----------------------|
| UI States | 15 states (separate flows) | 6 states (unified flow) |
| Architecture | Explicit data fetching states | Event-driven via CustomEvents |
| Error States | Multiple error states | Single ERROR state |

### Benefits of Simplified Approach

1. **Easier to maintain** — Fewer states = less complexity
2. **More flexible** — Event system enables component decoupling
3. **Better performance** — Async updates do not block UI
4. **Graceful degradation** — Failures handled invisibly

---

## 1. UI State Machine

```mermaid
stateDiagram-v2
    [*] --> INITIAL

    INITIAL --> SEARCHING : search_start
    INITIAL --> SEARCHING : geolocation_start

    SEARCHING --> RESULTS_RELEVANCE : success_with_results
    SEARCHING --> NO_RESULTS : success_no_results
    SEARCHING --> ERROR : search_error

    RESULTS_RELEVANCE --> RESULTS_DISTANCE : sort_changed
    RESULTS_DISTANCE --> RESULTS_RELEVANCE : sort_changed

    RESULTS_RELEVANCE --> SEARCHING : new_search
    RESULTS_DISTANCE --> SEARCHING : new_search
    NO_RESULTS --> SEARCHING : new_search
    ERROR --> SEARCHING : retry
    ERROR --> INITIAL : dismiss
```

### State Descriptions

| State | Description | Visual Indicators |
|-------|-------------|-------------------|
| `INITIAL` | Page loaded, no search | Empty input, regional map view |
| `SEARCHING` | Search in progress | Loading indicator visible |
| `RESULTS_RELEVANCE` | Results grouped by type | Section headers, sorted by ranking |
| `RESULTS_DISTANCE` | Results sorted by distance | Flat list, closest first |
| `NO_RESULTS` | No results found | Empty state message |
| `ERROR` | Search failed | Error message with retry |

### Key Implementation Details

- **Unified search flow**: Text search and geolocation use the same states
- **Search mode tracked internally**: Flag distinguishes search types
- **Error consolidation**: All errors use single state with different messages
- **Async distance calculation**: Happens after state transition

---

## 2. Map State Machine

The map is **display-only** — it responds to search results but does NOT trigger searches.

```mermaid
stateDiagram-v2
    [*] --> MAP_DEFAULT

    MAP_DEFAULT --> MAP_CENTRED : results_received
    MAP_CENTRED --> MAP_CENTRED : user_pan_or_zoom
    MAP_CENTRED --> MAP_CENTRED : new_results

    state MAP_CENTRED {
        [*] --> MARKERS_DISPLAYED
        MARKERS_DISPLAYED --> MARKER_HIGHLIGHTED : card_hover
        MARKER_HIGHLIGHTED --> MARKERS_DISPLAYED : card_unhover
        MARKERS_DISPLAYED --> TOOLTIP_OPEN : marker_clicked
        TOOLTIP_OPEN --> MARKERS_DISPLAYED : tooltip_closed
    }
```

**Key Principle**: Panning/zooming the map does NOT affect the results list.

---

## 3. Tooltip State Machine

```mermaid
stateDiagram-v2
    [*] --> TOOLTIP_CLOSED

    TOOLTIP_CLOSED --> TOOLTIP_OPEN : marker_clicked
    TOOLTIP_OPEN --> TOOLTIP_CLOSED : close_clicked
    TOOLTIP_OPEN --> TOOLTIP_CLOSED : escape_key
    TOOLTIP_OPEN --> TOOLTIP_CLOSED : click_outside
    TOOLTIP_OPEN --> TOOLTIP_OPEN : different_marker_clicked
```

### Accessibility

Implements APG Non-Modal Dialog Pattern:

- `role="dialog"` with `aria-labelledby`
- Focus moves to dialog on open
- Escape key closes
- Focus returns to trigger on close

---

## 4. Information Overlay State Machine

```mermaid
stateDiagram-v2
    [*] --> OVERLAY_CLOSED

    OVERLAY_CLOSED --> OVERLAY_OPEN : info_link_clicked
    OVERLAY_CLOSED --> OVERLAY_OPEN_HIGHLIGHTED : header_info_clicked

    OVERLAY_OPEN --> OVERLAY_CLOSED : close_clicked
    OVERLAY_OPEN --> OVERLAY_CLOSED : escape_key

    OVERLAY_OPEN_HIGHLIGHTED --> OVERLAY_CLOSED : close_clicked
    OVERLAY_OPEN_HIGHLIGHTED --> OVERLAY_CLOSED : escape_key
```

When opened from a results header, the corresponding section is highlighted.

---

## 5. Distance Calculation Flow

Distance calculation is handled asynchronously via events:

```mermaid
sequenceDiagram
    participant Overview
    participant DistanceService
    participant API
    participant Card

    Overview->>DistanceService: Calculate distances
    DistanceService->>API: Request (batched)
    
    loop Each batch
        API-->>DistanceService: Results
        DistanceService->>Card: Update event
        Card->>Card: Display distance
    end
    
    DistanceService->>Overview: Complete event
```

### Graceful Degradation

If distance calculation fails:

- Distance shows "—" instead of value
- No error shown to user
- Sorting falls back to straight-line distance

---

## 6. Sort Mode Behaviour

### Relevance Sort (Default)

1. Results rendered immediately, grouped by type
2. Distances calculated asynchronously
3. Cards update in-place as distances arrive

### Distance Sort

1. Loading state shown
2. All distances calculated
3. Results sorted and rendered
4. Falls back to straight-line if API fails

---

## 7. URL Synchronisation

```mermaid
stateDiagram-v2
    [*] --> CHECK_URL

    CHECK_URL --> RESTORE_SEARCH : has_params
    CHECK_URL --> INITIAL : no_params

    RESTORE_SEARCH --> RESULTS : success
    RESTORE_SEARCH --> INITIAL : failed

    RESULTS --> UPDATE_URL : search_complete
```

### URL Parameters

| Parameter | Description |
|-----------|-------------|
| `q` | Search query |
| `lat` | Latitude (geolocation) |
| `lng` | Longitude (geolocation) |
| `sort` | Sort mode |

---

## Summary

### Implementation Status

| Feature | Status |
|---------|--------|
| Text Search | ✅ Complete |
| Geolocation Search | ✅ Complete |
| Map Display | ✅ Complete |
| Results Sorting | ✅ Complete |
| Distance Calculation | ✅ Complete |
| URL Persistence | ✅ Complete |
| Map Tooltip | ✅ Complete |
| Error Handling | ✅ Complete |

### Key Patterns

1. **Event-driven communication** between components
2. **Async distance updates** that do not block rendering
3. **Graceful degradation** for API failures
4. **Unified state machine** for both search modes

---

*Document updated: January 2026*
