# Chapter 6: Memory Banks

---

Cursor forgets context between chat sessions. Each new conversation starts fresh. A memory bank solves this by giving Cursor a place to store and retrieve project information.

A memory bank is a folder of documentation files that Cursor reads at the start of each conversation and updates as you work. You do not need to maintain these files manually—Cursor does it based on your conversations.

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 6.1 | The three memory bank files | You want the overview first |
| 6.2 | Setting up for a new project | You already have an existing project |
| 6.3 | Setting up for an existing project | You are starting fresh |
| 6.4 | How files get populated | You understand the basics |
| 6.5 | Reviewing memory bank files | Do not skip—important for accuracy |
| 6.6 | Using memory banks in later sessions | You are still on first session |
| 6.7 | Guidelines | You want practical tips |
| 6.8 | Setup instructions | Quick setup steps |

---

## 6.1 The Three Files

Create a `memory-bank/` folder in your project with three files:

```
memory-bank/
├── project_brief.md      # What the project is about
├── data_dictionary.md    # All data files and variables
└── progress_log.md       # Running log of work done
```

| File | Purpose | Who updates it |
|------|---------|----------------|
| `project_brief.md` | Research question, background, approach, constraints | Cursor, based on your conversations |
| `data_dictionary.md` | Data files, variables, sources | Cursor, when you load or describe data |
| `progress_log.md` | What was done in each session | Cursor, after completing tasks |

---

## 6.2 Setting Up for a New Project

When you start a new project, have a conversation with Cursor about what you are trying to do. Do not just say "set up a new project"—share your thinking.

### Example: Starting a Project

**You:**
> I'm starting a research project and could use help thinking through the setup. I want to study how federal infrastructure spending affects regional employment. I have panel data for 50 US regions from 2015 to 2023—employment rates from BLS and grant amounts from a federal database.
>
> I'm thinking fixed effects might be right since I want to control for permanent regional differences, but I'm not sure about the details. My advisor mentioned clustering standard errors but I don't fully understand when that matters. The paper is due in about 6 weeks.
>
> Can you help me set up the project and ask me any questions about what I'm trying to do?

**Cursor:**
> This sounds like a well-defined empirical project. Before I set things up, a few questions:
>
> 1. Are you estimating the effect of spending levels, or changes in spending? (This affects whether you need region fixed effects, time fixed effects, or both.)
> 2. Do all regions receive grants every year, or do some have zeros?
> 3. Are there specific controls your advisor wants you to include?
> 4. Any journal formatting requirements I should know about?

**You:**
> Good questions. Spending varies by region and year—some regions get more in certain years based on federal allocations. Some have zeros in some years. My advisor wants me to control for education levels and GDP per capita. No specific journal yet, but standard economics formatting.

**Cursor:**
> Got it. Based on what you've described, I'll set up the project with:
> - A virtual environment for Python
> - A memory bank with your project details
> - Two-way fixed effects (region and year) as the baseline approach
> - Clustered standard errors at the region level (since that's where your treatment varies)
>
> I'll note the open questions about handling zeros and the 6-week timeline. Setting this up now...

Cursor creates the folder structure and populates the memory bank files with everything discussed.

---

## 6.3 Setting Up for an Existing Project

If you already have a project with data files, code, and results, you can still create a memory bank. Cursor can analyze what exists and document it.

### Example: Adding a Memory Bank to Existing Work

**You:**
> I have an existing research project in this folder that I've been working on without Cursor. There are several data files in data/, some analysis scripts in code/, and results in results/. I want to start using Cursor's memory bank feature.
>
> Can you look through what's here and help me create a memory bank? I can fill in details about things you can't infer from the files.

**Cursor:**
> I'll explore your project and create a memory bank. Let me look at your files...
>
> I found:
> - 3 CSV files in data/ (regional_panel.csv, grants.csv, merged_data.csv)
> - 4 Python scripts in code/
> - Several output files in results/
>
> A few questions to complete the memory bank:
> 1. What's the main research question you're investigating?
> 2. For regional_panel.csv, what does each row represent?
> 3. What stage is the project at—data cleaning, analysis, or writing?

**You:**
> The research question is whether infrastructure grants affect regional employment. Each row in regional_panel.csv is a region-year observation. I've finished cleaning and run a baseline regression, but I haven't done robustness checks yet.

**Cursor:**
> Got it. I'll create the memory bank with:
> - project_brief.md documenting your research question and current stage
> - data_dictionary.md with all your data files and variables
> - progress_log.md noting that baseline analysis is complete
>
> Creating these now...

Cursor examines your data files, infers variable information where possible, and asks about anything unclear.

---

## 6.4 How the Files Get Populated

You do not fill in templates. Cursor populates the files based on your conversations.

### project_brief.md

After the conversation in section 6.2 above, Cursor creates:

```markdown
# Project Brief

## Research Question
What is the effect of federal infrastructure spending on regional employment?

## Background and Motivation
Panel study using variation in federal grant allocations across 50 US regions
from 2015-2023. Interested in causal effect of spending levels on employment rates.

## Data Overview
- Employment data from BLS (regional employment rates)
- Infrastructure grants from federal database
- 50 regions, 9 years
- See data_dictionary.md for variable details

## Approach
- Two-way fixed effects (region and year)
- Standard errors clustered at region level
- Controls: education rate, GDP per capita

## Timeline and Constraints
- 6-week deadline
- Standard economics journal formatting
- Advisor requires education and GDP controls

## Open Questions
- How to handle regions with zero grants in some years
- Whether to include region-specific time trends
```

### data_dictionary.md

When you load data, Cursor adds entries:

```markdown
# Data Dictionary

## regional_panel.csv
- **Source**: Bureau of Labor Statistics
- **Unit of observation**: Region-year
- **Coverage**: 50 regions, 2015-2023 (450 observations)
- **Variables**:
  | Variable | Description | Type | Notes |
  |----------|-------------|------|-------|
  | region_id | Region identifier | string | |
  | year | Year | integer | 2015-2023 |
  | employment_rate | Employment-to-population ratio | numeric | Percentage |
  | education_rate | Post-secondary education share | numeric | Percentage |
  | gdp_per_capita | Real GDP per capita | numeric | Dollars |

## infrastructure_grants.csv
- **Source**: Federal grants database
- **Unit of observation**: Region-year
- **Coverage**: 50 regions, 2015-2023
- **Variables**:
  | Variable | Description | Type | Notes |
  |----------|-------------|------|-------|
  | region_id | Region identifier | string | |
  | year | Year | integer | |
  | infrastructure_spending | Grant amount | numeric | Millions of dollars |
```

### progress_log.md

After each work session, Cursor appends:

```markdown
# Progress Log

## 2024-01-15: Project Setup
**Task**: Initialize project structure and memory bank
**Actions**:
- Created virtual environment
- Set up memory-bank/ folder with project documentation
- Discussed fixed effects approach and clustering
**Outputs**: project_brief.md, data_dictionary.md populated
**Decisions**: Two-way FE with region clustering as baseline
**Next**: Load and merge data files

## 2024-01-16: Data Cleaning
**Task**: Merge employment and infrastructure data
**Actions**:
- Loaded regional_panel.csv and infrastructure_grants.csv
- Merged on region_id and year (left join)
- Created lagged infrastructure_spending variable
- Checked for missing values (89 in infrastructure_spending)
**Outputs**: data/regional_panel_clean.csv (447 rows after dropping missing employment)
**Decisions**: Keep missing infrastructure as-is (will be excluded from regressions using that variable)
**Next**: Summary statistics and baseline regression
```

---

## 6.5 Reviewing Memory Bank Files

**This section is important.** While Cursor maintains the memory bank, you must review it periodically to catch errors.

### Why Review Matters

Cursor infers information from your data files and conversations. It may:
- Misunderstand what a variable represents
- Record incorrect units (dollars vs. millions of dollars)
- Miss important details about data sources or limitations

If the memory bank contains errors, Cursor will use that incorrect information in future sessions. An error in the data dictionary can propagate into your analysis.

### What to Check

**data_dictionary.md** — Review this file carefully.
- Are variable descriptions accurate?
- Are units correct? (This affects coefficient interpretation)
- Are data sources properly noted?
- Are any variables missing?

**project_brief.md** — Verify the approach section.
- Does it reflect your current methodology?
- Are constraints and requirements accurate?

**progress_log.md** — Skim for accuracy.
- Were decisions recorded correctly?
- Is anything important missing?

### When to Review

- After Cursor first creates the memory bank
- After adding new data files
- After making significant methodological changes
- Before starting a new phase of the project

### How to Fix Errors

If you find an error:

**You:**
> I reviewed the data dictionary. The variable infrastructure_spending is described as "millions of dollars" but it's actually in thousands. Can you fix that?

**Cursor:**
> Updated data_dictionary.md. Changed infrastructure_spending units from "Millions of dollars" to "Thousands of dollars."

---

## 6.6 Using the Memory Bank in Later Sessions

When you return to the project in a new chat session, Cursor reads the memory bank and understands your context.

**You (new session, days later):**
> I'm back to work on the infrastructure project. Where did we leave off?

**Cursor:**
> Based on the progress log, you completed data cleaning on January 16. The merged dataset is in data/regional_panel_clean.csv with 447 observations. The baseline approach is two-way fixed effects with clustered standard errors.
>
> Next steps were summary statistics and baseline regression. Want me to proceed with that?

You do not need to re-explain the project. The memory bank provides continuity.

### Updating the Memory Bank

Cursor updates files automatically after significant work. You can also request updates:

**You:**
> Add to the project brief that we decided to exclude 2020-2021 from the main analysis due to COVID effects. We'll include them in a robustness check.

**Cursor:**
> Updated project_brief.md with the COVID exclusion decision under Approach.

---

## 6.7 Guidelines

**Share your thinking, not just instructions.** When you explain your reasoning and uncertainties, Cursor can ask clarifying questions and document decisions properly.

**Review the data dictionary after setup.** This is the most critical file for accuracy. Errors here affect all future work.

**Let the files grow.** The project brief should expand as the project evolves. New data files get added to the dictionary. The progress log becomes a complete record of the project.

**Do not duplicate.** If something is in your Global User Rules (like "use LaTeX for tables"), do not repeat it in the memory bank. Rules are for conventions; memory bank is for project-specific state.

---

## 6.8 Setup Instructions

There are two ways to set up a memory bank:

### Option A: Let Cursor Create It (Recommended)

1. **Install Global User Rules first**
   - Open `templates/global-user-rules.md` in this guide
   - Copy everything inside the code block
   - Paste into Cursor Settings → Rules → User Rules (see Chapter 5 for details)

2. **Start a conversation about your project**
   - Open your project folder in Cursor
   - In the chat, describe your project: your research question, data, timeline, and any uncertainties
   - Cursor will create the `memory-bank/` folder and populate the files based on your discussion

3. **Review what Cursor creates**
   - Check the data dictionary for accuracy
   - Verify the project brief reflects your approach

### Option B: Create It Manually

If you prefer to set up the structure yourself:

1. **Create the folder structure** in your project:
   ```
   your-project/
   └── memory-bank/
       ├── project_brief.md
       ├── data_dictionary.md
       └── progress_log.md
   ```

2. **Copy the template files** from `templates/memory-bank/` in this guide to your project's `memory-bank/` folder

3. **Fill in the files** as you work, or let Cursor update them during your conversations

---

[← Previous: Rules and Commands](05-customization.md) | [Next: Project Organization →](07-project-workflow.md)
