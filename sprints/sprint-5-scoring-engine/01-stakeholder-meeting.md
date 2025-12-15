# Stakeholder Meeting Minutes

**Meeting:** Sprint 5 Planning - Scoring Engine
**Attendees:** Marcus Thompson, Sarah Chen, David Park, Andrew

---

## Meeting Notes

### Marcus (Product Owner)

> "This is the heart of our product. Anyone can display data - we provide personalized rankings.
>
> **User Story:**
> 'As a user, I want to set my priorities (climate, jobs, cost, etc.) and see which locations best match my preferences.'
>
> **Scoring Categories:**
> 1. **Affordability** - Lower cost of living is better
> 2. **Job Market** - Lower unemployment, higher wages is better
> 3. **Climate** - Depends on preference (some want warm, some want seasons)
> 4. **Education** - Higher education levels = better
>
> **Key Insight:** Different metrics point in different directions:
> - High income = good
> - High home prices = bad (less affordable)
> - High unemployment = bad
>
> The scoring engine must handle this correctly."

---

### Sarah (Tech Lead)

> "**Normalization Strategy:**
>
> Convert everything to 0-100 where higher = better:
>
> ```python
> # For metrics where higher is better (income, sunny days)
> normalized = (value - min) / (max - min) * 100
>
> # For metrics where lower is better (home price, unemployment)
> normalized = (1 - (value - min) / (max - min)) * 100
> ```
>
> **Composite Score:**
> ```python
> score = (
>     affordability_score * affordability_weight +
>     job_score * job_weight +
>     climate_score * climate_weight +
>     education_score * education_weight
> )
> # Weights must sum to 1.0
> ```
>
> **API Design:**
> ```
> POST /api/v1/rankings
> {
>   \"weights\": {
>     \"affordability\": 0.3,
>     \"jobs\": 0.3,
>     \"climate\": 0.2,
>     \"education\": 0.2
>   },
>   \"climate_preference\": \"warm\",  // warm, cold, moderate
>   \"limit\": 20
> }
> ```"

---

### David (Senior Developer)

> "Implementation tips:
>
> 1. **Cache min/max values** - Don't recalculate for every request
> 2. **Handle NULL data** - Skip locations missing data, or use defaults
> 3. **Validate weights** - Must sum to 1.0
> 4. **Create a service class** - Keep scoring logic separate from API layer
> 5. **Unit test thoroughly** - The math must be correct"

---

## Scoring Metrics

### Affordability Score (0-100)
```python
# Higher score = more affordable
components = [
    invert_normalize(median_home_value),      # Lower is better
    invert_normalize(median_rent),            # Lower is better
    normalize(median_income)                  # Higher is better (buying power)
]
affordability_score = average(components)
```

### Job Market Score (0-100)
```python
components = [
    invert_normalize(unemployment_rate),      # Lower is better
    normalize(median_wage),                   # Higher is better
    normalize(job_growth_rate)                # Higher is better
]
job_score = average(components)
```

### Climate Score (0-100)
```python
# Depends on preference
if preference == "warm":
    score = normalize(avg_temp)               # Higher temp = higher score
elif preference == "cold":
    score = invert_normalize(avg_temp)        # Lower temp = higher score
else:  # moderate
    score = closeness_to(avg_temp, target=65) # Closer to 65 = higher score
```

### Education Score (0-100)
```python
score = normalize(bachelors_degree_pct)
```

---

*Next: Read `02-requirements.md`*
