# Definition of Done - Sprint 4

---

## Completion Checklist

### Power BI Setup ✓
- [ ] Power BI Desktop installed (or alternative tool)
- [ ] PostgreSQL connection working
- [ ] All four tables imported
- [ ] Data refreshes successfully

### Data Model ✓
- [ ] Relationships defined between all tables
- [ ] Cardinality correct (1:1 or 1:many)
- [ ] Model diagram screenshot saved
- [ ] Basic measures created

### Visualizations ✓
- [ ] Coverage dashboard page
- [ ] Demographics analysis page
- [ ] Employment analysis page
- [ ] Climate analysis page
- [ ] At least 5 distinct visualization types used

### Interactivity ✓
- [ ] State slicer works across pages
- [ ] Cross-filtering between visuals
- [ ] Consistent formatting and colors
- [ ] Clear titles and labels

### Documentation ✓
- [ ] Data quality report written (`docs/data-quality-report.md`)
- [ ] Insights document created (`docs/data-insights.md`)
- [ ] Journal entry for Sprint 4
- [ ] Key screenshots saved

---

## Data Quality Report Must Include

1. **Coverage Statistics**
   - Total locations count
   - Percentage with demographics
   - Percentage with employment
   - Percentage with climate

2. **Missing Data Analysis**
   - Which states have gaps
   - Any patterns identified
   - Severity assessment

3. **Data Issues**
   - Outliers discovered
   - Suspicious values
   - Recommendations for fixes

---

## Insights Document Must Include

1. **Key Findings**
   - At least 3 findings per data category
   - Supported by visualizations

2. **Patterns Identified**
   - Geographic patterns
   - Correlations between metrics
   - Regional differences

3. **API Recommendations**
   - Suggested endpoints
   - Null handling strategy
   - Filter priorities

---

## Verification Steps

### Test Data Refresh
1. Make a small change in PostgreSQL
2. Click Refresh in Power BI
3. Verify change appears

### Test Interactivity
1. Select a state in slicer
2. Verify all pages filter correctly
3. Click on visual element
4. Verify cross-filtering works

### Review Visualizations
- [ ] Do charts tell a clear story?
- [ ] Are axes labeled appropriately?
- [ ] Is color usage consistent?
- [ ] Are numbers formatted correctly?

---

## Sample Quality Check Queries

Run these in PostgreSQL to verify your Power BI numbers:

```sql
-- Total locations
SELECT COUNT(*) FROM locations;

-- Demographics coverage
SELECT
    COUNT(d.id) * 100.0 / COUNT(l.id) as demo_coverage
FROM locations l
LEFT JOIN demographics d ON l.id = d.location_id;

-- Employment coverage
SELECT
    COUNT(e.id) * 100.0 / COUNT(l.id) as emp_coverage
FROM locations l
LEFT JOIN employment e ON l.id = e.location_id;

-- Climate coverage
SELECT
    COUNT(c.id) * 100.0 / COUNT(l.id) as climate_coverage
FROM locations l
LEFT JOIN climate c ON l.id = c.location_id;

-- States with missing data
SELECT
    l.state_name,
    COUNT(l.id) as total_locations,
    COUNT(d.id) as has_demographics,
    COUNT(e.id) as has_employment,
    COUNT(c.id) as has_climate
FROM locations l
LEFT JOIN demographics d ON l.id = d.location_id
LEFT JOIN employment e ON l.id = e.location_id
LEFT JOIN climate c ON l.id = c.location_id
GROUP BY l.state_name
ORDER BY has_climate ASC
LIMIT 10;
```

---

## What Your Tech Lead Will Review

1. **Data Model Quality**
   - Are relationships correctly defined?
   - Is the model efficient?

2. **Visualization Effectiveness**
   - Do visuals communicate insights?
   - Is the design clean and professional?

3. **Analysis Depth**
   - Did you find meaningful patterns?
   - Are findings well-documented?

4. **Technical Execution**
   - Do measures calculate correctly?
   - Does interactivity work?

5. **Documentation**
   - Are reports complete and clear?
   - Would another developer understand?

---

## Sprint Sign-Off

Before moving to Sprint 5:

- [ ] All checklist items complete
- [ ] Power BI report saved (.pbix)
- [ ] Documentation committed to git
- [ ] Journal entry written
- [ ] Can explain 3 key insights discovered
- [ ] Know what API endpoints would be valuable

---

*Proceed to `../sprint-5-core-api/`*
