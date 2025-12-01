# Data Analysis Prompts

## Prompts for Exploring, Analyzing, and Understanding Data

Data analysis with AI combines human intuition about what questions matter with AI's ability to process and synthesize information quickly. These prompts help you extract insights from data.

---

## Exploratory Data Analysis

### Initial Data Exploration

```
Analyze this dataset and provide an initial exploration:

[paste data sample or describe the dataset]

Provide:
1. Summary statistics for each column
2. Data types and potential issues (nulls, inconsistencies)
3. Distribution descriptions for key variables
4. Obvious patterns or anomalies
5. Suggested visualizations
6. Initial questions worth investigating

Format findings clearly with sections for each area.
```

### Data Quality Assessment

```
Assess the quality of this data:

[paste data sample]

Check for:
1. Missing values - how many, which columns, any patterns?
2. Duplicates - exact and near-duplicates
3. Inconsistencies - format variations, contradictory values
4. Outliers - extreme values that may be errors
5. Data type issues - numbers as strings, date formats
6. Logical errors - impossible values, violated constraints

Provide a quality score and prioritized list of issues to address.
```

### Pattern Discovery

```
Find patterns in this data:

[paste data or describe it]

Look for:
1. Correlations between variables
2. Temporal patterns (if time data exists)
3. Clusters or segments
4. Unusual combinations
5. Sequences or progressions

For each pattern found:
- Describe the pattern
- Quantify its strength
- Suggest possible explanations
- Note caveats or limitations
```

---

## Statistical Analysis

### Descriptive Statistics

```
Calculate and interpret descriptive statistics for:

[paste data]

For numerical variables:
- Mean, median, mode
- Standard deviation and variance
- Quartiles and IQR
- Skewness and kurtosis

For categorical variables:
- Frequency counts
- Proportions
- Mode

Interpret what these statistics tell us about the data in plain language.
```

### Hypothesis Testing

```
Help me test this hypothesis:

Hypothesis: [state your hypothesis]
Data: [paste or describe data]

Provide:
1. Appropriate statistical test and why
2. Assumptions to check
3. Test execution (calculations or code)
4. Results interpretation
5. Effect size and practical significance
6. Confidence interval
7. Plain language conclusion

Be explicit about limitations and what we can/cannot conclude.
```

### Regression Analysis

```
Perform regression analysis on this data:

[paste data]

Dependent variable: [target]
Independent variables: [predictors]

Provide:
1. Model specification and assumptions
2. Coefficient estimates with interpretation
3. Model fit metrics (R-squared, etc.)
4. Residual analysis
5. Prediction for sample inputs
6. Limitations and caveats

Explain results for someone with basic statistics knowledge.
```

---

## Business Analysis

### Metric Analysis

```
Analyze this business metric:

Metric: [name and definition]
Data: [paste time series or comparison data]
Context: [what this metric measures and why it matters]

Provide:
1. Current state summary
2. Trend analysis
3. Comparison to benchmarks or targets
4. Anomaly identification
5. Contributing factors (if data available)
6. Actionable insights
7. Recommended next steps

Format for executive presentation.
```

### Cohort Analysis

```
Perform cohort analysis on this user/customer data:

[paste data with user IDs, dates, and metrics]

Analyze:
1. Define cohorts by [signup date/first purchase/etc.]
2. Track [retention/revenue/engagement] over time
3. Compare cohort performance
4. Identify best and worst performing cohorts
5. Suggest reasons for differences
6. Recommend actions based on findings

Visualize as a cohort table with clear interpretation.
```

### Funnel Analysis

```
Analyze this conversion funnel:

Funnel stages:
[list stages with counts]

Calculate:
1. Conversion rate at each step
2. Overall funnel conversion
3. Biggest drop-off points
4. Comparison to industry benchmarks (if known)
5. Estimated impact of improving each step

Provide specific, actionable recommendations for improvement.
```

---

## Log and Event Analysis

### Log Pattern Analysis

```
Analyze these logs for patterns and issues:

[paste log sample]

Look for:
1. Error patterns and frequencies
2. Performance issues (slow operations)
3. Unusual sequences of events
4. Security concerns
5. Resource usage patterns

Summarize findings with:
- Issue severity ranking
- Frequency counts
- Recommended investigation areas
- Suggested alerts to set up
```

### User Behavior Analysis

```
Analyze user behavior from this event data:

[paste event stream or describe data]

Discover:
1. Common user journeys
2. Feature usage patterns
3. Drop-off points
4. Power user vs. casual user behaviors
5. Time-based patterns (day of week, session length)

Provide insights about:
- What users are trying to accomplish
- Where they struggle
- Opportunities for improvement
```

---

## Comparative Analysis

### A/B Test Analysis

```
Analyze this A/B test:

Test: [what was tested]
Control group: [metrics]
Treatment group: [metrics]
Sample sizes: [n for each group]
Duration: [how long test ran]

Calculate:
1. Conversion rates for each group
2. Absolute and relative difference
3. Statistical significance (p-value)
4. Confidence interval
5. Required sample size check
6. Recommendation (ship/don't ship/extend test)

Include caveats about the analysis.
```

### Competitive Analysis

```
Compare these options based on the data:

Options: [list options]
Data: [paste comparison data]
Decision criteria: [what matters]

Provide:
1. Feature-by-feature comparison
2. Scoring matrix
3. Pros and cons for each option
4. Recommendation with reasoning
5. Scenarios where different options win

Present objectively, acknowledge trade-offs.
```

---

## Data Synthesis

### Summarize Large Dataset

```
Summarize this large dataset into key findings:

[paste or describe dataset]

I need:
1. Executive summary (3-5 bullet points)
2. Key metrics and their values
3. Most important trends
4. Anomalies or concerns
5. Recommended actions

Keep the summary concise but complete enough to make decisions.
```

### Multi-Source Data Synthesis

```
Synthesize insights from these different data sources:

Source 1: [paste or describe]
Source 2: [paste or describe]
Source 3: [paste or describe]

Provide:
1. Common themes across sources
2. Contradictions or conflicts
3. Unique insights from each source
4. Overall narrative that emerges
5. Gaps in understanding
6. Recommended next analysis steps
```

---

## Code Generation for Analysis

### Generate Analysis Code

```
Write code to analyze this data:

Data description: [describe the data structure]
Analysis needed: [what you want to learn]
Language: [Python/R/SQL]
Libraries: [pandas/tidyverse/etc.]

Include:
1. Data loading and cleaning
2. Exploratory analysis
3. Main analysis steps
4. Visualization code
5. Results interpretation comments

Write clear, commented code that others can understand.
```

### SQL Query for Analysis

```
Write a SQL query to answer this question:

Question: [your analysis question]
Tables available:
[describe tables and columns]

Requirements:
- Optimized for readability
- Comments explaining logic
- Handle edge cases (nulls, etc.)
- Include any useful intermediate CTEs
```

---

## Quick Reference

### Analysis Checklist

```
Before presenting analysis, verify:

[ ] Data source is reliable and understood
[ ] Sample size is adequate
[ ] Methodology is appropriate
[ ] Results are reproducible
[ ] Limitations are acknowledged
[ ] Conclusions follow from evidence
[ ] Actionable recommendations included
```

### Common Pitfalls

1. **Correlation vs. causation** - Association doesn't prove cause
2. **Selection bias** - Is the sample representative?
3. **Survivorship bias** - Are we only seeing successes?
4. **Cherry-picking** - Are we ignoring contradictory data?
5. **p-hacking** - Multiple comparisons increase false positives

---

*Data tells stories, but stories need interpretation. Use these prompts to find the signal in the noise and translate data into understanding.*
