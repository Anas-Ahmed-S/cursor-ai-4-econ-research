# Chapter 7: Project Organization and Workflow

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 7.1 | Example folder structure | You have an existing structure you prefer |
| 7.2 | Version control basics | You already use Git |
| 7.3 | Integrating Cursor into your workflow | You want a quick overview of when to use Cursor |
| 7.4 | Collaborating with coauthors | You are working solo |
| 7.5 | Complete workflow example | You want to see a full project from start to finish |

---

This chapter covers how to organize research projects for effective use with Cursor, integrate Cursor into your research workflow, and collaborate with coauthors.

---

## 7.1 Example Folder Structure

A consistent folder structure helps Cursor understand your project and helps you maintain organization over time. The structure below is one example; adapt it to your needs. Empirical projects, theoretical papers, and survey-based research may require different layouts.

### Example Layout

```
project/
├── data/
│   ├── raw/                    # Original, unmodified data files
│   └── processed/              # Cleaned and prepared data
├── code/
│   ├── 01_data_cleaning.py
│   ├── 02_analysis.py
│   └── 03_visualization.py
├── results/
│   ├── tables/                 # Regression tables, summary stats
│   └── figures/                # Charts and plots
├── memory-bank/
│   ├── project_brief.md
│   ├── data_dictionary.md
│   └── progress_log.md
├── .cursor/rules/
└── README.md
```

### Why This Structure Helps

| Folder | Purpose |
|--------|---------|
| `data/raw/` | Preserves original files; never modify these |
| `data/processed/` | Contains cleaned data that analysis scripts read |
| `code/` | Numbered scripts indicate execution order |
| `results/` | Separates outputs from inputs for clarity |
| `memory-bank/` | Provides Cursor with persistent project context |

---

## 7.2 Version Control Basics

Version control tracks changes to your files over time. Git is the most widely used system.

### Why Version Control Matters with Cursor

Agent Mode can modify multiple files at once. If something goes wrong, version control allows you to:

- See exactly what changed
- Revert to a previous version
- Experiment with changes safely

### Essential Git Commands

**Initialize a repository:**
```bash
git init
```

**Check status:**
```bash
git status
```

**Stage and commit changes:**
```bash
git add .
git commit -m "Add baseline regression specification"
```

**View history:**
```bash
git log --oneline
```

**Revert to previous commit:**
```bash
git checkout [commit-hash] -- filename.py   # Restore specific file
git checkout .                               # Discard all uncommitted changes
```

### Commit Before Agent Mode

Before asking Cursor to make significant changes via Agent Mode, commit your current work:

```bash
git add .
git commit -m "Checkpoint before Agent Mode changes"
```

**Common Mistake:** Running Agent Mode without saving or committing first. If Agent Mode modifies files in undesired ways, you may have difficulty recovering your previous state without version control.

---

## 7.3 Integrating Cursor into Your Workflow

### When to Use Cursor

Cursor is most helpful for:

- **Routine coding tasks**: Loading data, reshaping, merging
- **Methodological guidance**: Discussing which approach fits your research question
- **Syntax lookup**: Forgetting the exact pandas or statsmodels function
- **Generating boilerplate**: Creating script templates, output formatting
- **Exploration**: Understanding unfamiliar code or trying new methods

### When to Apply Your Own Judgment

You remain responsible for:

- **Understanding your data**: Knowing what variables mean and how they were collected
- **Interpreting results**: Determining whether estimates are plausible
- **Defending your choices**: Being able to explain why you chose a specification

### A Typical Session

1. **Discuss your goal** with Cursor—share what you are trying to do and any uncertainties
2. **Let Cursor ask questions** to clarify details
3. **Agree on an approach** before implementation
4. **Have Cursor implement** the agreed approach
5. **Check outputs** to verify results make sense
6. **Commit when satisfied**

---

## 7.4 Collaborating with Coauthors

### Sharing Cursor Configuration

To ensure consistent behavior across team members:

1. **Include `.cursor/rules/` in version control**: This shares Project Rules
2. **Share `memory-bank/`**: Provides project context to all collaborators
3. **Document User Rules**: Each team member should configure similar User Rules for consistency

### Onboarding New Collaborators

When a new coauthor joins:

1. Have them clone the repository
2. Direct them to read `memory-bank/project_brief.md`
3. Explain the folder structure and naming conventions
4. Ensure they configure appropriate User Rules

### Avoiding Conflicts

- **Assign ownership of files**: Each person edits specific scripts
- **Communicate before Agent Mode**: If you plan to use Agent Mode on shared files, inform coauthors
- **Review merge conflicts carefully**: AI-generated code may conflict with collaborators' changes

---

## 7.5 Complete Workflow Example

This section walks through starting a new research project using the conversational approach.

### Step 1: Start the Conversation

Open a new project folder in Cursor and have a discussion:

**You:**
> I'm starting a research project studying the effect of federal infrastructure spending on regional employment. I have panel data for 50 US regions from 2015-2023—employment rates from BLS and grant amounts from a federal database.
>
> I'm planning to use fixed effects but I'm not entirely sure about all the details. Can you help me set up the project structure and ask me any questions about what I'm trying to do?

**Cursor:**
> This sounds like a well-structured empirical project. Before setting up, a few questions:
>
> 1. What controls do you plan to include?
> 2. Any specific timeline or deadlines?
> 3. Will you be collaborating with others on this project?

After discussing, Cursor sets up:
- Virtual environment
- Folder structure
- Memory bank with your project details documented

### Step 2: Initialize Version Control

```bash
git init
git add .
git commit -m "Initial project structure"
```

### Step 3: Work on Data Cleaning

Have a conversation about your data:

**You:**
> I have two CSV files that need to be merged: regional_panel.csv with employment data and infrastructure_grants.csv with spending data. Some regions have missing grant data in certain years. Can you help me think through how to clean and merge these?

Cursor asks about merge keys, how to handle missing values, what derived variables you need. After agreeing on an approach, it creates the cleaning script.

Review the output (row counts, missing values) and commit:

```bash
git add .
git commit -m "Add data cleaning script"
```

### Step 4: Run Analysis

**You:**
> The cleaned data looks good. Now I want to run the baseline regression we discussed—two-way fixed effects with clustered standard errors. Can you set that up and also show me summary statistics first?

Cursor generates the analysis script. You check that coefficient magnitudes and signs make sense, then commit.

### Step 5: Iterate

Continue working through visualizations, robustness checks, and refinements. The memory bank tracks your progress automatically. Each significant change gets committed.

---

[← Previous: Memory Banks](06-memory-banks.md)
