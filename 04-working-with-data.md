# Chapter 4: Working with Data

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 4.1 | The workflow | You understand the basic pattern |
| 4.2 | Asking for advice on data cleaning | You know how to clean your data |
| 4.3 | Choosing a visualization approach | You know what figures you need |
| 4.4 | Unsure about econometric approach | You already know your methodology |
| 4.5 | Executing a known task | You want guidance, not execution |
| 4.6 | Checking your outputs | Do not skip—important for catching errors |
| 4.7 | Asking for robustness checks | You are still on baseline analysis |
| 4.8 | When something looks wrong | You want troubleshooting examples |
| 4.9 | Summary | Quick reference table |

---

This chapter demonstrates how to use Cursor for common research tasks: cleaning data, creating visualizations, and running regressions. The approach is conversational—you describe what you want, Cursor asks clarifying questions, you agree on a plan, and Cursor executes it.

You do not need to write code yourself or understand every line Cursor produces. Your job is to clearly describe your goal, answer Cursor's questions, and verify that the outputs make sense.

---

## 4.1 The Workflow

For any data task, follow this pattern:

1. **Describe your goal** — Tell Cursor what you want to accomplish in plain language
2. **Answer questions** — Cursor may ask about your data, preferences, or edge cases
3. **Review the plan** — Cursor proposes a plan; you confirm or adjust it
4. **Execute** — Cursor writes and runs the code
5. **Check outputs** — Verify the results make sense (not the code itself)

This workflow applies whether you are cleaning data, making charts, or running regressions.

Sometimes you know exactly what you want. Other times, you are unsure which method to use. In both cases, start by describing your situation—Cursor can advise on methodology before executing.

---

## 4.2 Example: Asking for Advice on Data Cleaning

You have messy data and are not sure how to handle it.

### The Conversation

**You:**
> I scraped data from government websites on infrastructure projects. I have about 5,000 records but it's messy—some entries have missing values, there are duplicate project IDs, and the date formats are inconsistent. I need to clean this for analysis but I'm not sure where to start. What approach would you recommend?

**Cursor:**
> Let me help you think through this. For messy scraped data, I'd suggest this sequence:
>
> 1. **First, assess the damage**: Look at how many duplicates, missing values, and format issues you have. This tells us how much cleaning is needed.
>
> 2. **Handle duplicates**: Are they true duplicates (same project listed twice) or different records that happen to share an ID? We need to decide: keep first, keep last, or flag for manual review.
>
> 3. **Standardize dates**: What formats are you seeing? (MM/DD/YYYY, YYYY-MM-DD, written months?) I can detect and convert these.
>
> 4. **Address missing values**: Depends on the variable. For critical IDs, we might need to drop. For numeric fields, we could impute or leave missing.
>
> Want me to start by loading the data and showing you a diagnostic summary? That will help us decide on the specific approach.

**You:**
> Yes, let's see the diagnostic first.

**Cursor:**
> Here's what I found:
>
> - Total records: 5,247
> - Duplicate project_ids: 312 (affecting 624 rows)
> - Missing values: project_amount (89), start_date (156), region (23)
> - Date formats detected: 3 different formats
>
> For duplicates, I checked a few—they appear to be the same project entered twice, not different projects. I recommend keeping the most recent entry for each.
>
> For missing project_amount, that's only 1.7% of data. We could either drop those rows or keep them and let the regression handle it (they'll be excluded from analyses using that variable).
>
> For dates, I can standardize all to YYYY-MM-DD format.
>
> Does this approach sound right, or would you prefer something different?

**You:**
> That sounds good. But for the missing project_amount, can you check if they're missing randomly or if there's a pattern? Like, are they all from the same region or year?

**Cursor:**
> Good question. Let me check...
>
> The 89 missing project_amount values are concentrated:
> - 67 are from 2015 (the first year of data)
> - 18 are from Region 12
> - 4 are scattered
>
> This suggests the early data collection may have been incomplete, and Region 12 might have reporting issues. You might want to note this as a data limitation rather than assuming random missingness.
>
> Should I proceed with cleaning, keeping these observations but flagging them?

**You:**
> Yes, proceed. Create a flag variable for the missing amounts so I can check robustness later.

---

## 4.3 Example: Choosing a Visualization Approach

You have results but are unsure how to present them.

### The Conversation

**You:**
> I've run my regression and I want to visualize the results, but I'm not sure what kind of figure would be most effective. The main finding is that infrastructure spending has a positive effect on employment, and the effect varies by region type (urban vs rural). What would you suggest?

**Cursor:**
> There are several options depending on what you want to emphasize:
>
> **Option 1: Coefficient plot**
> Shows point estimates with confidence intervals. Good for: comparing the effect across urban vs rural regions side by side. Clean and common in economics papers.
>
> **Option 2: Marginal effects plot**
> Shows how the predicted employment rate changes as infrastructure spending increases, with separate lines for urban and rural. Good for: showing the economic magnitude and the difference between groups visually.
>
> **Option 3: Binned scatter plot**
> Divide infrastructure spending into bins, plot average employment for each bin, separately for urban and rural. Good for: showing the raw relationship in the data without imposing linearity.
>
> Which aspect is most important for your paper? The statistical comparison (coefficient plot), the magnitude of the effect (marginal effects), or the pattern in the data (binned scatter)?

**You:**
> I think the marginal effects plot would be good—I want readers to see how the effect differs between urban and rural regions in a way that's easy to interpret.

**Cursor:**
> Good choice. A few more questions:
>
> 1. What range of infrastructure spending should the x-axis cover? (The observed range in your data, or a policy-relevant range like $0-50 million?)
> 2. Should I include confidence bands around the lines?
> 3. Any specific formatting requirements for your target journal?

**You:**
> Use the observed range in the data. Yes, include 95% confidence bands. Keep it simple, black and white, suitable for an economics journal.

**Cursor:**
> I'll create a marginal effects plot with:
> - X-axis: infrastructure spending (observed range)
> - Y-axis: predicted employment rate
> - Two lines: urban (solid) and rural (dashed)
> - Shaded 95% confidence bands for each
> - Black/gray color scheme
>
> Proceed?

---

## 4.4 Example: Unsure About Econometric Approach

You want to estimate an effect but are not sure which method is appropriate.

### The Conversation

**You:**
> I want to estimate the effect of infrastructure spending on employment, but I'm not sure about the right econometric approach. I have panel data—50 regions over 9 years. I've heard about fixed effects and difference-in-differences but I'm not clear on which applies to my situation. Can you help me think through this?

**Cursor:**
> Let me ask some questions to help identify the right approach:
>
> 1. **Is there a specific policy change or event?** Difference-in-differences is ideal when some regions receive a "treatment" at a specific time and others don't. Is infrastructure spending a sudden policy change, or does it vary continuously across regions and years?
>
> 2. **What's your main concern about bias?** Are you worried that regions with higher spending are systematically different (e.g., richer regions spend more and have higher employment anyway)?
>
> 3. **Does spending vary within regions over time, or mostly between regions?** This affects which fixed effects structure makes sense.

**You:**
> 1. It's not a single policy event—spending varies across regions and years based on federal grant allocations.
> 2. Yes, I'm worried that regions that get more grants might already have different employment trends.
> 3. It varies both between regions and within regions over time. Some regions got more in certain years.

**Cursor:**
> Based on this, I'd recommend a **two-way fixed effects** approach:
>
> - **Region fixed effects** control for time-invariant differences between regions (e.g., Region A is always more urban than Region B)
> - **Year fixed effects** control for common shocks affecting all regions (e.g., 2020 was bad everywhere due to COVID)
>
> This approach asks: "Within a given region, when it received more infrastructure spending than usual, did employment increase more than usual?"
>
> The key assumption is that there are no region-specific time trends that correlate with both spending and employment. If you're worried about that, we could add robustness checks with region-specific trends.
>
> An alternative would be instrumental variables if you have a valid instrument, but that requires a source of variation in spending that is unrelated to employment trends.
>
> Do you want to proceed with two-way fixed effects as the baseline?

**You:**
> Yes, let's do two-way fixed effects. But can you explain what clustering standard errors means? My advisor mentioned I should do that.

**Cursor:**
> Standard errors measure how precise your estimates are. The default calculation assumes each observation is independent. But in panel data, observations from the same region are likely correlated—if Region A has high employment in 2015, it probably also has high employment in 2016.
>
> **Clustering at the region level** adjusts the standard errors to account for this correlation within regions. It typically makes standard errors larger (more conservative), which means you're less likely to claim statistical significance when it's actually just noise.
>
> With 50 regions, clustering is appropriate and you have enough clusters for reliable inference. I'll include this in the regression.
>
> Ready to run the model?

---

## 4.5 Example: Executing a Known Task

Sometimes you know exactly what you need. Here, the conversation is shorter—you specify, Cursor confirms details, and executes.

### The Conversation

**You:**
> I have two datasets I need to merge. The first is regional_panel.csv with employment and demographic data for 50 regions from 2015-2023. The second is infrastructure_grants.csv with federal grant amounts. I want to merge them on region_id and year, keep all observations from the panel, and create a lagged infrastructure spending variable. Can you help?

**Cursor:**
> Got it. Here's my plan:
>
> 1. Load regional_panel.csv and infrastructure_grants.csv
> 2. Merge using a left join on region_id and year (keeps all panel rows)
> 3. Sort by region and year, then create infrastructure_spending_lag1
> 4. Create log_gdp_per_capita (handling any zero/negative values)
> 5. Drop rows where employment_rate is missing
> 6. Save the cleaned data to data/regional_panel_clean.csv
> 7. Print summary: row counts before/after, missing values by variable
>
> Should I proceed?

**You:**
> Yes, go ahead.

### What to Check in the Output

After Cursor executes, look at the summary it prints:

```
Original panel: 450 rows
After merge: 450 rows
After dropping missing employment_rate: 447 rows

Missing values:
  employment_rate: 0
  infrastructure_spending: 23
  infrastructure_spending_lag1: 50
  education_rate: 5
  log_gdp_per_capita: 0

Saved to: data/regional_panel_clean.csv
```

**What to verify:**
- Row count after merge matches your expectation (450 = 50 regions × 9 years)
- The 23 missing infrastructure_spending values make sense (regions with no grants)
- The 50 missing lag values are expected (first year of each region has no prior year)
- No unexpected drops occurred

You do not need to read the code line by line. If the numbers look right, the cleaning worked.

---

## 4.6 Checking Your Outputs

After Cursor executes any task, verify the results make sense. You do not need to read the code—focus on the outputs.

### For Data Cleaning

- **Row counts**: Do they match your expectation? (e.g., 50 regions × 9 years = 450)
- **Missing values**: Are they where you expect? (e.g., first year has no lag)
- **Variable summaries**: Do means and ranges look plausible?

### For Visualizations

- **Trends**: Do patterns look reasonable? Sudden jumps may indicate errors.
- **Scales**: Are axes in the right units? (Employment should be 50-70%, not 0.5 or 5000)
- **Labels**: Are titles and axis labels correct?

### For Regressions

- **Coefficient magnitude**: Is the effect size plausible given the units?
- **Signs**: Do controls have expected signs? (More education → higher employment)
- **Sample size**: Does N match your cleaned data?
- **Significance**: Are standard errors reasonable relative to coefficients?

If something looks wrong, ask Cursor to investigate:
> "The coefficient seems too large. Can you check the units of infrastructure_spending?"

---

## 4.7 Asking for Robustness Checks

Once your baseline results look reasonable, ask for robustness checks:

**You:**
> The baseline looks good. Can you run some robustness checks? I want to try:
> 1. Using lagged infrastructure spending instead of current
> 2. Dropping 2020-2021 because of COVID
> 3. Adding population as an additional control
>
> Put all four specifications (baseline plus three checks) in one table.

**Cursor:**
> I'll create a table with four columns:
> - (1) Baseline
> - (2) Lagged spending
> - (3) Excluding 2020-2021
> - (4) Adding population control
>
> All with the same fixed effects and clustering. I'll save to results/tables/robustness.tex
>
> Proceed?

### What to Check

Compare coefficients across specifications:
- Do they stay roughly similar? (Good—results are robust)
- Does the coefficient flip signs or change by a factor of 10? (Problem—investigate why)
- Does excluding COVID years dramatically change results? (Interesting—may need to discuss)

---

## 4.8 When Something Looks Wrong

If outputs do not make sense:

**Problem: Unexpected row counts**
> "The merge dropped 200 observations. I expected to keep all 450. Can you show me which region-years were dropped and why?"

**Problem: Implausible coefficient**
> "The coefficient on infrastructure_spending is 5.2, which would mean a $1 million grant raises employment by 5 percentage points. That seems too large. Can you check the units of infrastructure_spending? Is it in millions or raw dollars?"

**Problem: Missing values where you did not expect them**
> "The regression only used 300 observations but my cleaned data has 447 rows. What's causing the additional drops?"

**Problem: Figure looks strange**
> "The time series shows employment jumping from 60% to 95% in 2019. That can't be right. Can you check if there's a data error or coding issue for that year?"

Cursor will investigate and often find the issue—a unit mismatch, a merge gone wrong, or a data entry error.

---

## 4.9 Summary

| Situation | What you do | What Cursor does |
|-----------|-------------|------------------|
| Unsure about method | Describe your goal and data | Explains options, recommends approach |
| Know what you need | Specify the task | Confirms details, executes |
| Results look wrong | Describe what seems off | Investigates, finds the issue |

The key insight: Cursor is a knowledgeable collaborator. When you are unsure, ask for advice. When you know what you need, describe it clearly. Either way, your job is to verify that outputs make sense—not to read every line of code.

---

[← Previous: Effective Prompts](03-effective-prompts.md) | [Next: Rules and Commands →](05-customization.md)
