# Technical Guidance - Sprint 7

**From:** Sarah Chen (Tech Lead)
**Subject:** Frontend Features & Visualization Design Specifications

---

## Overview

This document provides design specifications for the advanced frontend features including maps, charts, and the rankings interface. You'll need to research the libraries and write the code yourself.

---

## New Dependencies

Install these packages:
- `leaflet` - Map rendering library
- `react-leaflet` - React wrapper for Leaflet
- `@types/leaflet` - TypeScript types (dev dependency)
- `chart.js` - Charting library
- `react-chartjs-2` - React wrapper for Chart.js

Research each library's documentation for installation and setup.

---

## Map Integration

### Leaflet Basics

Leaflet renders interactive maps using "tiles" from a tile provider (like OpenStreetMap).

**Key Concepts:**
- **Map container** - The div that holds the map
- **Tile layer** - The map image tiles (streets, terrain, etc.)
- **Markers** - Points on the map
- **Popups** - Info boxes that appear on click

### components/Map.tsx Requirements

Create a location map component:

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| locations | Location[] | Locations to display as markers |
| onLocationClick | (id: number) => void | Handler when marker is clicked |
| center? | [number, number] | Map center coordinates |
| zoom? | number | Initial zoom level |

**Default Values:**
- Center: US center approximately [39.8283, -98.5795]
- Zoom: 4 (shows entire US)

**Requirements:**
1. Render MapContainer with specified center and zoom
2. Add TileLayer using OpenStreetMap tiles
3. For each location with valid lat/lon:
   - Render a Marker at coordinates
   - Attach a Popup with location name and state
   - Add "View Details" button in popup that calls onLocationClick
4. Skip locations without coordinates

**Leaflet CSS:**
Don't forget to import Leaflet's CSS file for proper styling.

Research: react-leaflet documentation, OpenStreetMap tile URLs

---

## Chart.js Integration

### Chart.js Setup

Chart.js requires explicit registration of components you use.

**Components to Register:**
- `CategoryScale` - For X-axis labels
- `LinearScale` - For Y-axis numbers
- `BarElement` - For bar charts
- `RadialLinearScale` - For radar charts
- `PointElement` - For radar chart points
- `LineElement` - For radar chart lines
- `Filler` - For filled areas
- `Title`, `Tooltip`, `Legend` - For chart features

Research: Chart.js registration, tree-shaking

### components/charts/BarChart.tsx Requirements

Create a bar chart component:

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| labels | string[] | X-axis labels |
| data | number[] | Y-axis values |
| title? | string | Chart title |
| colors? | string[] | Bar colors |

**Default Colors:**
Provide an array of 4-5 distinct colors for different bars.

**Chart Data Structure:**
Research the Chart.js data format - it uses `labels` and `datasets` arrays.

### components/charts/RadarChart.tsx Requirements

Create a radar/spider chart for comparing locations:

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| locations | LocationScores[] | Array of locations with scores |
| labels | string[] | Category labels (Affordability, Jobs, etc.) |

**LocationScores Type:**
```
name: string
scores: number[]  // Scores in same order as labels
```

**Requirements:**
- Support comparing 2-4 locations on one chart
- Use different colors for each location
- Show filled areas with transparency

Research: Chart.js radar chart configuration

---

## Rankings Page Implementation

### pages/Rankings.tsx Requirements

The main feature page where users find their ideal locations.

**State to Manage:**

| State | Type | Default |
|-------|------|---------|
| weights | WeightsObject | Equal weights (25 each) |
| climatePreference | string | "moderate" |
| stateFilter | string | "" (empty = all states) |
| results | RankingResult[] | [] |
| loading | boolean | false |

**WeightsObject:**
```
affordability: number (0-100)
jobs: number (0-100)
climate: number (0-100)
education: number (0-100)
```

### Weight Sliders Feature

**Challenge:** When one slider changes, others should adjust to maintain sum of 100%.

**Approach Options:**
1. **Lock and redistribute** - Lock some sliders, redistribute among unlocked
2. **Proportional adjustment** - Reduce/increase others proportionally
3. **Simple validation** - Just warn if sum ≠ 100

Research and implement one of these approaches.

**UI Elements Needed:**
- Four range sliders (0-100)
- Labels showing current value with %
- Visual indicator if sum ≠ 100

### Climate Preference Selector

Dropdown with options:
- "Warm Climate"
- "Cold Climate"
- "Moderate Climate"

### State Filter (Optional)

Dropdown to filter results by state:
- "All States" option
- List of all US states

### Submit Button

Calls the rankings API with:
- Weights converted to decimals (divide by 100)
- Climate preference
- State filter (if selected)
- Limit (e.g., 20)

### Results Display

After fetching rankings, display:
- Numbered list (1-20)
- Location name and state
- Composite score
- Category breakdown (all four scores)
- Click to view location details

Consider adding:
- Bar chart showing top results
- Option to compare selected results

---

## Compare Page Implementation

### pages/Compare.tsx Requirements

Allow users to compare 2-4 locations side by side.

**State to Manage:**

| State | Type | Description |
|-------|------|---------|
| selectedLocations | Location[] | Locations to compare (max 4) |
| searchQuery | string | Search input value |
| searchResults | Location[] | Search results to choose from |

### Location Selection

**UI Flow:**
1. User types in search box
2. Show search results as selectable list
3. User clicks result to add to comparison
4. Show selected locations with remove button
5. Limit to 4 locations maximum

### Comparison Display

For selected locations, show:
- Side-by-side cards with key metrics
- Radar chart comparing all scores (if scores calculated)
- Table with all data points

**Key Metrics to Compare:**
- Population
- Median household income
- Median home value
- Median rent
- Unemployment rate
- Climate data

---

## API Service Extensions

### services/api.ts Additions

Add rankings service methods:

#### `rankingService`

| Method | Parameters | Returns | Endpoint |
|--------|------------|---------|----------|
| getRankings | RankingRequest | RankingResult[] | POST /api/v1/rankings |

**RankingRequest Type:**
```
weights: {
  affordability: number
  jobs: number
  climate: number
  education: number
}
climate_preference: "warm" | "cold" | "moderate"
state_filter?: string
limit: number
```

**RankingResult Type:**
```
location_id: number
fips_code: string
name: string
state: string
affordability_score: number
job_score: number
climate_score: number
education_score: number
composite_score: number
```

---

## Custom Hooks for Features

### hooks/useRankings.ts

Hook to manage rankings state and API calls:
- Manages weights, preference, filter state
- Provides handleSubmit function
- Returns results and loading state

### hooks/useCompare.ts

Hook to manage comparison state:
- Tracks selected locations
- Provides add/remove functions
- Provides search function

---

## UI/UX Considerations

### Responsive Design

All new features should work on:
- Desktop (full layout)
- Tablet (adjusted grid)
- Mobile (stacked layout)

Use Tailwind's responsive prefixes: `md:`, `lg:`

### Loading States

Show loading indicators during:
- Rankings calculation
- Search operations
- Data fetching

### Error Handling

Display user-friendly messages for:
- API errors
- No results found
- Invalid inputs

### Accessibility

- Use semantic HTML elements
- Add aria-labels to interactive elements
- Ensure keyboard navigation works

---

## Testing Strategy

### Manual Testing

**Map Component:**
1. Verify markers appear at correct locations
2. Test popup open/close
3. Test View Details button
4. Test with missing coordinates

**Rankings Page:**
1. Test slider interactions
2. Verify weights sum handling
3. Test API call with various inputs
4. Verify results display correctly

**Compare Page:**
1. Test search and selection
2. Test remove functionality
3. Verify 4-location limit
4. Test radar chart rendering

### Cross-Browser Testing

Test in:
- Chrome
- Firefox
- Safari (if on Mac)

---

## Research Topics

1. **Leaflet** - https://leafletjs.com/
2. **react-leaflet** - https://react-leaflet.js.org/
3. **Chart.js** - https://www.chartjs.org/
4. **react-chartjs-2** - https://react-chartjs-2.js.org/
5. **Range inputs in React** - Controlled components with sliders
6. **Responsive design** - Tailwind responsive utilities

---

## Definition of "Done" for Code Review

When I review your code, I'll check:

1. Does the map display locations correctly?
2. Do charts render properly?
3. Do weight sliders work intuitively?
4. Does the rankings API call work?
5. Does comparison work for multiple locations?
6. Is the UI responsive?
7. Are loading/error states handled?

---

*Next: Read `05-developer-tasks.md`*
