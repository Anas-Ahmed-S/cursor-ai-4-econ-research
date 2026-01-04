# Global User Rules for Cursor

This file contains rules that apply to all your Cursor projects. Follow the instructions at the end of this file to install them.

---

## Rules to Copy

Copy everything inside the code block below (from `# Language and Libraries` to `# Output Standards`):

```
# Language and Libraries
- Use Python with pandas, statsmodels, linearmodels, and matplotlib
- Write clear, readable code with meaningful variable names
- Use snake_case for variables, functions, and file names (e.g., employment_rate, load_data, data_cleaning.py)
- Number scripts to indicate execution order when appropriate (e.g., 01_load_data.py, 02_clean_data.py)
- Include comments explaining econometric reasoning, not just code mechanics

# Communication Style
- Be direct and concise
- Use formal academic language
- When explaining econometric concepts, assume graduate-level economics knowledge
- Flag potential issues with identification or data quality
- When the user shares detailed thoughts or uncertainties, engage with them—ask clarifying questions before proceeding

# Project Setup
When starting a new project, ask the user about:
1. Their research question and what they are trying to understand
2. What data they have or plan to use
3. Their timeline or constraints
4. Any requirements (journal style, advisor preferences, methods they want to use)

Then create:
1. A Python virtual environment: python -m venv venv
2. A memory-bank/ folder with three files:
   - project_brief.md
   - data_dictionary.md
   - progress_log.md
3. A README.md file in the project root

# README.md
Create and maintain a README.md file that explains:
- Project title and research question (one sentence)
- Folder structure and what each folder contains
- Data files and their sources (brief—details are in data_dictionary.md)
- How to run the analysis (which scripts to run in what order)
- Key outputs and where to find them

Update README.md when:
- New folders or significant files are added
- The analysis workflow changes
- New data sources are added

Keep it concise—one to two pages maximum. This is for quick orientation, not detailed documentation.

# Memory Bank: project_brief.md
This file captures everything about the project. Structure it as:

## Research Question
[One clear statement of what the user is trying to understand or estimate]

## Background and Motivation
[Why this question matters, what the user has shared about context]

## Data Overview
[Brief description of datasets—details go in data_dictionary.md]

## Approach
[Current thinking on methods, which may evolve]

## Timeline and Constraints
[Deadlines, requirements, advisor preferences]

## Open Questions
[Things still being figured out]

Update this file as the project evolves. Add sections as needed. This should grow into a comprehensive project reference.

# Memory Bank: data_dictionary.md
When the user mentions or loads a data file, document it:

## [filename.csv]
- **Source**: Where the data comes from
- **Unit of observation**: What each row represents
- **Coverage**: Time period, geographic scope, sample size
- **Variables**:
  | Variable | Description | Type | Notes |
  |----------|-------------|------|-------|
  | var_name | What it measures | numeric/string/date | Any issues or transformations |

Add entries for each data file. Update when variables are created or transformed.

# Memory Bank: progress_log.md
After completing significant work, append an entry:

## [Date]: [Brief title]
**Task**: What was requested
**Actions**: What was done (bullet points)
**Outputs**: Files created or modified
**Decisions**: Any choices made and why
**Next**: What remains or follows from this

Keep entries concise but informative. This is a running log, not a summary.

# Context Awareness
At the start of each conversation:
- Read files in memory-bank/ to understand project context
- Check for project rules in `.cursor/rules/` for project-specific conventions
- If memory-bank exists, briefly acknowledge the project context

# Output Standards
- Regression tables: Export to LaTeX format
- Figures: Save as PNG at 300 DPI
- Always print row counts after merges and data transformations
- Include sanity checks (summary statistics, expected ranges)
```

---

## How to Install These Rules

1. **Open Cursor Settings**:
   - Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows) to open the command palette
   - Type "Cursor Settings" and select it
   - Alternatively, click the gear icon in the bottom-left corner

2. **Navigate to Rules**: In the Settings sidebar, click on **Rules**

3. **Find User Rules**: You will see a text area labeled "User Rules" where you can enter rules that apply to all projects

4. **Paste the rules**: Copy everything inside the code block above (from `# Language and Libraries` through `# Output Standards`) and paste it into the User Rules text area

5. **Close Settings**: Your rules are saved automatically

These rules now apply to all your projects. For project-specific rules, create rule files in `.cursor/rules/` in each project folder (see Chapter 5).
