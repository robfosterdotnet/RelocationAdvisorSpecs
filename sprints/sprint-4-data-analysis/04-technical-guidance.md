# Technical Guidance - Sprint 4

**From:** Sarah Chen (Tech Lead)
**Subject:** Power BI Setup and Analysis Patterns

---

## Overview

This document provides guidance for setting up Power BI Desktop and connecting to your PostgreSQL database. You'll need to research specific features and experiment with the tool.

---

## Installing Power BI Desktop

### Download
1. Visit: https://powerbi.microsoft.com/desktop/
2. Download the free Desktop version
3. Install (Windows required - see Mac alternatives below)

### Mac Users
Power BI Desktop is Windows-only. Options:
1. **Windows VM** - Run Windows in Parallels/VMware/VirtualBox
2. **Boot Camp** - Dual-boot Windows on Mac
3. **Power BI Service** - Limited free tier at app.powerbi.com
4. **Alternative Tools** - Metabase (open source), Tableau Public, or Apache Superset

If using an alternative, adapt the guidance to that tool's interface.

---

## Connecting to PostgreSQL

### Prerequisites
- PostgreSQL server running locally
- Database with data from previous sprints
- Network access (localhost usually works)

### Connection Steps
1. **Get Data** > **Database** > **PostgreSQL database**
2. Server: `localhost`
3. Database: `relocation_advisor` (or your database name)
4. Data Connectivity mode: **Import** (recommended for performance)
5. Enter credentials when prompted

### Troubleshooting
- If connection fails, verify PostgreSQL is running
- Check `pg_hba.conf` allows local connections
- Ensure you're using the correct port (default: 5432)

---

## Building the Data Model

### Import These Tables
```
locations
demographics
employment
climate
```

### Creating Relationships

In the Model view:
1. Drag `locations.id` to `demographics.location_id`
2. Drag `locations.id` to `employment.location_id`
3. Drag `locations.id` to `climate.location_id`

### Relationship Properties
- **Cardinality:** One to One (if each location has max one record per table)
- **Cross filter direction:** Both (for bidirectional filtering)
- **Make this relationship active:** Yes

### Data Model Pattern
```
                    ┌──────────────┐
                    │  locations   │
                    │     (id)     │
                    └──────┬───────┘
                           │
           ┌───────────────┼───────────────┐
           │               │               │
    ┌──────▼─────┐  ┌──────▼─────┐  ┌──────▼─────┐
    │demographics│  │ employment │  │  climate   │
    │(location_id)│ │(location_id)│ │(location_id)│
    └────────────┘  └────────────┘  └────────────┘
```

---

## Creating Measures with DAX

### What is DAX?
Data Analysis Expressions - Power BI's formula language. Similar to Excel formulas but more powerful.

### Essential Measures to Create

**Total Locations:**
```dax
Total Locations = COUNTROWS(locations)
```

**Demographics Coverage:**
```dax
Demographics Coverage % =
DIVIDE(
    COUNTROWS(demographics),
    COUNTROWS(locations),
    0
) * 100
```

**Employment Coverage:**
```dax
Employment Coverage % =
DIVIDE(
    COUNTROWS(employment),
    COUNTROWS(locations),
    0
) * 100
```

**Climate Coverage:**
```dax
Climate Coverage % =
DIVIDE(
    COUNTROWS(climate),
    COUNTROWS(locations),
    0
) * 100
```

**Average Income:**
```dax
Avg Household Income = AVERAGE(demographics[median_household_income])
```

**Average Unemployment:**
```dax
Avg Unemployment Rate = AVERAGE(employment[unemployment_rate])
```

### Creating Measures
1. Select a table in the Fields pane
2. Click "New Measure" in the Modeling tab
3. Enter the DAX formula
4. Press Enter

---

## Visualization Types to Use

### For Coverage Analysis
- **Card** - Single number (total count, percentage)
- **Table** - List of states with missing data
- **Stacked bar** - Coverage breakdown by category

### For Distributions
- **Histogram** - Distribution of numeric values
- **Box plot** - Quartile visualization (available in AppSource)
- **Scatter plot** - Two variables compared

### For Geographic Data
- **Filled map** - Color counties by metric
- **Shape map** - More precise geographic boundaries
- Note: Need state/county names or FIPS codes

### For Comparisons
- **Clustered bar** - Compare categories
- **Grouped column** - Multiple series
- **Table/Matrix** - Detailed comparisons

### For KPIs
- **Card** - Single important number
- **Gauge** - Progress toward target
- **KPI** - Value vs. target with trend

---

## Creating a Map Visualization

### Using Filled Map
1. Add "Filled Map" visual
2. Drag `state_name` to Location field
3. Drag a measure to Color saturation
4. Format: Choose appropriate color scheme

### County-Level Mapping
County-level maps are more complex:
- May need latitude/longitude columns
- Or use FIPS codes with appropriate map
- Consider using state-level for simplicity

---

## Slicers and Cross-Filtering

### Adding a State Slicer
1. Add "Slicer" visual
2. Drag `state_name` from locations table
3. Format as dropdown or list
4. Sync across pages (View > Sync slicers)

### Cross-Filtering Behavior
- Click on a visual element to filter others
- Ctrl+click to multi-select
- Configure in Format > Edit interactions

---

## Report Organization

### Recommended Page Structure
```
Page 1: Overview Dashboard
  - Key metrics cards
  - Coverage summary
  - State slicer

Page 2: Data Coverage Details
  - Coverage by state
  - Missing data analysis
  - Quality issues

Page 3: Demographics Analysis
  - Income distribution
  - Income map
  - Population insights

Page 4: Employment Analysis
  - Unemployment distribution
  - Job market map
  - Wage analysis

Page 5: Climate Analysis
  - Temperature distribution
  - Climate zones
  - Precipitation patterns
```

---

## Exporting Findings

### Screenshots
- Use Print Screen or Snipping Tool
- Save key visuals as images
- Include in documentation

### Export Data
- Right-click visual > Export data
- Useful for detailed analysis

### Save Report
- File > Save As
- Creates `.pbix` file
- Commit to git (note: large file)

---

## Research Topics

1. **DAX functions** - CALCULATE, FILTER, ALL, etc.
2. **Power BI visualizations** - Custom visuals from AppSource
3. **Report themes** - Consistent formatting
4. **Bookmarks** - Saved views and navigation
5. **Tooltips** - Rich hover information

---

## Definition of "Done" for Code Review

When I review your work, I'll check:

1. Can you refresh data from PostgreSQL?
2. Are table relationships correct?
3. Do measures calculate correctly?
4. Are visualizations appropriate for the data?
5. Does cross-filtering work?
6. Have you documented your findings?

---

*Next: Read `05-developer-tasks.md`*
