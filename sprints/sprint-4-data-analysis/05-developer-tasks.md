# Developer Tasks - Sprint 4

---

## Day 1: Setup and Data Model

### Installation
- [ ] Download Power BI Desktop from https://powerbi.microsoft.com/desktop/
- [ ] Install and launch (Windows required - see alternatives for Mac)
- [ ] Complete initial setup wizard

### Database Connection
- [ ] Ensure PostgreSQL is running (`pg_isready`)
- [ ] Get Data > PostgreSQL database
- [ ] Connect to `localhost` and your database
- [ ] Import all four tables: locations, demographics, employment, climate

### Data Model
- [ ] Switch to Model view
- [ ] Create relationship: locations.id → demographics.location_id
- [ ] Create relationship: locations.id → employment.location_id
- [ ] Create relationship: locations.id → climate.location_id
- [ ] Verify cardinality is correct (1:1 or 1:many)
- [ ] Take screenshot of model diagram

### Basic Measures
- [ ] Create "Total Locations" measure
- [ ] Create coverage percentage measures for each table
- [ ] Test measures return expected values

---

## Day 2: Coverage Analysis

### Coverage Dashboard (Page 1)
- [ ] Add Card visual for Total Locations
- [ ] Add Card visual for Demographics Coverage %
- [ ] Add Card visual for Employment Coverage %
- [ ] Add Card visual for Climate Coverage %
- [ ] Add Table showing states with lowest coverage
- [ ] Add Bar chart showing coverage by state

### Data Quality Analysis
- [ ] Identify states with missing demographics
- [ ] Identify states with missing employment data
- [ ] Identify states with missing climate data
- [ ] Note any patterns (geographic, systematic)
- [ ] Check for obvious data errors (negative values, outliers)

### Add Interactivity
- [ ] Add State slicer
- [ ] Test cross-filtering between visuals
- [ ] Add clear/reset filter button

---

## Day 3: Demographics and Employment Dashboards

### Demographics Dashboard (Page 2)
- [ ] Create income histogram (bin values)
- [ ] Create state-level income map
- [ ] Add average income card
- [ ] Add income range (min/max) cards
- [ ] Create scatter plot: income vs. population
- [ ] Add state slicer (synced with other pages)

### Employment Dashboard (Page 3)
- [ ] Create unemployment rate histogram
- [ ] Create state-level unemployment map
- [ ] Add average unemployment card
- [ ] Create Top 10 lowest unemployment table
- [ ] Create Top 10 highest unemployment table
- [ ] If wage data available, add wage visualizations

### Format and Polish
- [ ] Consistent color scheme across pages
- [ ] Clear titles for all visuals
- [ ] Appropriate number formatting (%, $, etc.)
- [ ] Remove unnecessary visual clutter

---

## Day 4: Climate Dashboard and Insights

### Climate Dashboard (Page 4)
- [ ] Create temperature distribution histogram
- [ ] Create temperature map by state
- [ ] Create precipitation analysis visuals
- [ ] Create climate comparison (hot vs. cold regions)
- [ ] Add relevant slicers and filters

### Cross-Analysis Dashboard (Page 5) - Optional
- [ ] Scatter: Income vs. Climate
- [ ] Scatter: Unemployment vs. Cost of Living
- [ ] Identify interesting correlations
- [ ] Highlight outliers or anomalies

### Final Interactivity
- [ ] Sync all slicers across pages
- [ ] Test navigation between pages
- [ ] Create bookmarks for key views
- [ ] Add navigation buttons (optional)

---

## Day 5: Documentation and Wrap-up

### Data Quality Report
Create a markdown file `docs/data-quality-report.md`:
```markdown
# Data Quality Report

## Overall Coverage
- Total Locations: X
- Demographics: X% coverage
- Employment: X% coverage
- Climate: X% coverage

## Missing Data Patterns
- [Describe geographic patterns]
- [Describe any systematic gaps]

## Outliers Identified
- [List any suspicious values]

## Data Errors Found
- [List any errors discovered]

## Recommendations
- [What data should be improved?]
```

### Insights Document
Create `docs/data-insights.md`:
```markdown
# Data Insights Report

## Key Findings

### Demographics
- [What did you learn about income distribution?]
- [Any geographic patterns?]

### Employment
- [What did you learn about job markets?]
- [Which regions are strongest/weakest?]

### Climate
- [Climate distribution patterns?]
- [Regional climate zones?]

## Correlations Discovered
- [Any interesting relationships between metrics?]

## API Design Recommendations
- [What endpoints would be valuable?]
- [How should we handle missing data?]
- [What filters would users want?]

## Scoring Engine Suggestions
- [Which metrics vary the most?]
- [What normalization approaches make sense?]
```

### Final Steps
- [ ] Save Power BI report as `.pbix`
- [ ] Export key visuals as screenshots
- [ ] Update `JOURNAL.md` with sprint reflections
- [ ] Commit all files to git

### Git Commit
```bash
# Note: .pbix files can be large
git add docs/data-quality-report.md
git add docs/data-insights.md
git add JOURNAL.md
# Consider adding .pbix to .gitignore if too large
git commit -m "feat: add data analysis and Power BI visualizations"
git push
```

---

## Verification Checklist

Before moving on, verify:

- [ ] Power BI can refresh data from PostgreSQL
- [ ] All four tables have relationships defined
- [ ] At least 5 dashboard pages created
- [ ] State slicer filters all pages
- [ ] Data quality report documents coverage gaps
- [ ] Insights document captures key findings
- [ ] Journal entry reflects on learning

---

*After completion, proceed to `../sprint-5-core-api/`*
