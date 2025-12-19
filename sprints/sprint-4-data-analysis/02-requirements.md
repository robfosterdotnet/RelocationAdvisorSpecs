# Requirements Document - Sprint 4

**Author:** Sarah Chen (Tech Lead)

---

## Power BI Setup Requirements

### FR-4.1: Tool Installation
- Install Power BI Desktop (free version)
- Configure for local PostgreSQL connection
- Verify data refresh capability

### FR-4.2: Database Connection
- Direct connection to PostgreSQL
- Import mode (for performance)
- All tables accessible

---

## Data Model Requirements

### FR-4.3: Relationships
- Define relationships between tables
- Locations as the central fact table
- Demographics, Employment, Climate as related dimensions
- Proper cardinality (one-to-one or one-to-many)

### FR-4.4: Calculated Fields
Create measures for:
- Total location count
- Coverage percentages per data type
- Average/median calculations
- Regional aggregations

---

## Dashboard Requirements

### FR-4.5: Data Coverage Dashboard
| Visual | Purpose |
|--------|---------|
| Card | Total locations count |
| Card | Demographics coverage % |
| Card | Employment coverage % |
| Card | Climate coverage % |
| Table | States with incomplete data |
| Bar chart | Coverage by state |

### FR-4.6: Demographics Dashboard
| Visual | Purpose |
|--------|---------|
| Map | Income by county |
| Histogram | Income distribution |
| Card | Average income |
| Scatter | Income vs. population |
| Slicer | State filter |

### FR-4.7: Employment Dashboard
| Visual | Purpose |
|--------|---------|
| Map | Unemployment by county |
| Histogram | Unemployment distribution |
| Bar chart | Top 10 highest/lowest |
| Card | National average |
| Trend | (if historical data available) |

### FR-4.8: Climate Dashboard
| Visual | Purpose |
|--------|---------|
| Map | Average temperature by county |
| Histogram | Temperature distribution |
| Scatter | Temperature vs. precipitation |
| Grouped bar | Climate by region |

### FR-4.9: Interactive Features
- State slicer (filter all pages)
- Cross-filtering between visuals
- Drill-through to location details
- Bookmarks for key views

---

## Analysis Output Requirements

### FR-4.10: Data Quality Report
Document the following:
1. Overall data completeness percentage
2. Missing data patterns (geographic/systematic)
3. Outliers identified
4. Data type issues (if any)
5. Recommendations for data improvement

### FR-4.11: Insights Document
Summarize:
1. Key findings about US locations
2. Interesting correlations discovered
3. Regional patterns identified
4. Recommendations for scoring weights
5. API design considerations

---

## Acceptance Criteria

1. Power BI Desktop installed and working
2. PostgreSQL connection functional
3. Data model with all relationships defined
4. At least 5 dashboard pages created
5. Cross-filtering works correctly
6. Data quality report completed
7. Insights document written
8. .pbix file saved and committed

---

*Next: Read `03-user-stories.md`*
