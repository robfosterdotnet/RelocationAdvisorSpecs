# Requirements Document - Sprint 3

**Author:** Sarah Chen (Tech Lead)

---

## BLS Pipeline Requirements

### FR-3.1: BLS Data Integration
- Fetch unemployment rate by county
- Fetch median wage data (may be at MSA level)
- Fetch job growth rate (year-over-year)
- Handle area code mappings

### FR-3.2: Employment Table Population
- Match to locations by FIPS
- Handle missing data gracefully
- Set appropriate data_year

---

## Climate Pipeline Requirements

### FR-3.3: Climate Data Integration
- Average annual temperature
- Summer high / Winter low
- Annual precipitation
- Annual snowfall
- Sunny days (if available)

### FR-3.4: Climate Table Population
- Map weather stations to counties (or use county-level data)
- Handle missing data gracefully

---

## Data Variables

### Employment Table
| Field | Source | Notes |
|-------|--------|-------|
| unemployment_rate | BLS LAUS | County level |
| median_hourly_wage | BLS OES | May be MSA level |
| job_growth_rate | BLS CES | Calculated from 2 years |
| labor_force | BLS LAUS | County level |

### Climate Table
| Field | Source | Notes |
|-------|--------|-------|
| avg_temp_annual | NOAA Normals | 30-year average |
| avg_temp_summer_high | NOAA Normals | July average |
| avg_temp_winter_low | NOAA Normals | January average |
| precipitation_annual | NOAA Normals | Inches |
| snowfall_annual | NOAA Normals | Inches |
| sunny_days | NOAA Normals | Clear sky days |

---

## Acceptance Criteria

1. Employment table has data for 2,500+ counties
2. Climate table has data for 2,500+ counties
3. Data passes validation checks
4. Scripts are idempotent
5. Documentation complete

---

*Next: Read `03-user-stories.md`*
