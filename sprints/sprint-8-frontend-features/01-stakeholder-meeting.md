# Stakeholder Meeting Minutes

**Meeting:** Sprint 8 Planning - Visualization Features
**Attendees:** Marcus Thompson, Sarah Chen, Jennifer Walsh, Andrew

---

## Jennifer (UX Designer)

> "This sprint brings the data to life with visuals:
>
> **Map View:**
> - Show US map with county markers
> - Click marker to see location info
> - Color-code by score (optional)
>
> **Rankings Page:**
> - Sliders for each preference weight
> - Real-time re-ranking as sliders move
> - Climate preference dropdown
>
> **Comparison View:**
> - Select 2-4 locations
> - Side-by-side data tables
> - Radar chart comparing scores
>
> **Location Detail:**
> - All data beautifully displayed
> - Charts for context (bar chart of scores)
> - Position relative to averages"

---

## Sarah (Tech Lead)

> "**Implementation Notes:**
>
> **Leaflet for Maps:**
> - Free, well-documented
> - Use react-leaflet wrapper
> - Markers cluster for performance
>
> **Chart.js for Charts:**
> - Use react-chartjs-2 wrapper
> - Bar chart for score breakdown
> - Radar chart for comparisons
>
> **Performance:**
> - Don't render 3,000 markers at once
> - Use clustering or limit visible markers
> - Consider server-side filtering for map bounds"

---

## Key Features

### Rankings Page Flow
1. User adjusts sliders (affordability, jobs, climate, education)
2. Sliders must sum to 100%
3. Select climate preference
4. Click "Find Locations"
5. See ranked results with scores

### Comparison Flow
1. Add locations to compare (from explore or search)
2. View side-by-side
3. See radar chart overlay
4. Clear visual winner per category

---

*Next: Read `02-requirements.md`*
