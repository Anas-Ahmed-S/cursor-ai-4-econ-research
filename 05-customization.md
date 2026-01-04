# Chapter 5: Rules and Commands

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 5.1 | Rules (User and Project) | You want to use Cursor without customization first |
| 5.2 | Commands | You do not need reusable prompt templates |
| 5.3 | Summary | Quick reference for all features |

---

Cursor can be configured to follow your preferences automatically. This chapter covers Rules (persistent instructions) and Commands (reusable prompts).

---

## 5.1 Rules

Rules are standing instructions that tell Cursor how to behave. They apply to every conversation without you having to repeat them.

### User Rules (Global)

User Rules apply to all your projects. Set them once, and Cursor follows them everywhere.

**Location:** Cursor Settings → Rules (look for "User Rules" text box)

**What to include:**
- Preferred programming language and libraries
- Coding style (variable naming, comments)
- Output preferences (file formats, figure resolution)
- Communication tone

**Example:**

```
- Use Python with pandas, statsmodels, and matplotlib
- Use snake_case for variable names
- Include comments explaining econometric reasoning
- Export tables to LaTeX format
- Save figures as PNG at 300 DPI
```

A complete User Rules template is provided in `templates/global-user-rules.md`. This template instructs Cursor to automatically set up project infrastructure when starting new projects. See Chapter 6 for details on memory banks.

### Project Rules

Project Rules apply only to a specific project. They override or extend User Rules for that project.

**Location:** Create files in the `.cursor/rules/` folder in your project. Each rule is a separate `.mdc` file.

```
project/
├── .cursor/
│   └── rules/
│       └── project-context.mdc
├── data/
├── code/
└── ...
```

**What to include:**
- Data file locations and names
- Variable definitions specific to this project
- Econometric specifications being used
- Output folder structure

**Example `.cursor/rules/project-context.mdc` for the Regional Employment Study:**

```
---
description: Project context for infrastructure and employment study
alwaysApply: true
---

# Project: Infrastructure and Employment

## Data
- Panel data: data/regional_panel.csv (50 regions, 2015-2023)
- Grants data: data/infrastructure_grants.csv
- Merge on: region_id, year

## Variables
- Outcome: employment_rate
- Treatment: infrastructure_spending
- Controls: education_rate, gdp_per_capita, population

## Methods
- Fixed effects: region and year
- Standard errors: clustered at region level

## Output
- Tables: results/tables/
- Figures: results/figures/
```

When you open this project, Cursor reads these rules and applies them automatically.

**Note:** Older projects may use a `.cursorrules` file in the project root. This legacy format still works but is deprecated. **Use the `.cursor/rules/` folder system for all new projects.** The new system offers more flexibility—you can have multiple rule files, scope rules to specific file patterns, and include file references.

---

## 5.2 Commands

Commands are reusable prompt templates. Instead of typing a long prompt each time, you invoke a command by name.

### User Commands (Global)

Available across all projects for tasks you do frequently.

**Location:** `~/.cursor/commands/` folder in your home directory (create this folder if it does not exist)

**To create a command:**
1. Create a folder: `~/.cursor/commands/`
2. Add a Markdown file for each command (e.g., `summarize_paper.md`)
3. The filename becomes the command name

**Example: Summarize a research paper**

Create a file `~/.cursor/commands/summarize_paper.md` with this content:

```
Summarize this economics paper:
1. Research question (one sentence)
2. Identification strategy
3. Main data source
4. Key finding
```

**Usage:** Type `/summarize_paper` in chat, then paste the abstract.

### Project Commands

Available only within a specific project for project-specific tasks.

**Location:** `.cursor/commands/` folder in your project directory

**To create a command:**
1. Create a folder: `your-project/.cursor/commands/`
2. Add a Markdown file for each command
3. Type `/` in chat to see available commands

**Example: Generate baseline regression**

Create a file `.cursor/commands/baseline_regression.md` with this content:

```
Generate a Python script that:
- Loads data/regional_panel_clean.csv
- Estimates: employment_rate ~ infrastructure_spending + education_rate + gdp_per_capita
- Region and year fixed effects
- Clustered standard errors at region level
- Exports to results/tables/baseline.tex
```

**Usage:** Type `/baseline_regression` in chat. Cursor generates the script using project-specific details.

---

## 5.3 Summary

| Feature | Scope | Created by | Purpose |
|---------|-------|------------|---------|
| User Rules | Global | You (in settings) | Default behavior for all projects |
| Project Rules | Project | You (`.cursor/rules/` folder) | Project-specific conventions |
| User Commands | Global | You (in settings or home `.cursor/commands/`) | Reusable prompts for common tasks |
| Project Commands | Project | You (project `.cursor/commands/`) | Reusable prompts for this project |

---

[← Previous: Working with Data](04-working-with-data.md) | [Next: Memory Banks →](06-memory-banks.md)
