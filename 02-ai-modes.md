# Chapter 2: AI Modes in Cursor

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 2.1 | Ask Mode | You only want to execute code, not learn |
| 2.2 | Agent Mode | You only want explanations, not file changes |
| 2.3 | Agent Mode security settings | You want to configure auto-run and sandboxing |
| 2.4 | Plan Mode | You want to start coding immediately |
| 2.5 | Choosing the right mode | You already know which mode to use |
| 2.6 | Providing context to Cursor | You understand how to reference files |
| 2.7 | Working with chat sessions | You understand how conversations work |

---

Cursor provides three modes for interacting with the AI. Each mode serves a different purpose:

| Mode | Purpose | Modifies Files? |
|------|---------|-----------------|
| Ask | Get explanations and guidance | No |
| Agent | Create and edit files | Yes (with permission) |
| Plan | Design a structured approach before coding | No (until executed) |

---

## 2.1 Ask Mode

Use Ask Mode when you want to understand something without changing any files.

**When to use:**
- Understanding what existing code does
- Learning how an econometric method works
- Getting help with Python syntax
- Deciding between statistical approaches

**Example prompts:**

> "Explain how fixed effects control for unobserved heterogeneity in panel data."

> "What is the difference between clustering standard errors at the state level versus the individual level?"

> "How do I reshape data from wide to long format in pandas?"

**What happens:** Cursor responds in the chat panel. It may show code snippets as illustrations, but nothing is saved to your project files.

**Common Mistake:** Using Ask Mode when you want code written into a file. If you ask "Write a script to run my regression," Cursor shows the code in chat but does not create a file. Switch to Agent Mode for file creation.

---

## 2.2 Agent Mode

Use Agent Mode when you want Cursor to create files, edit code, or run commands.

**When to use:**
- Creating new analysis scripts
- Editing existing code
- Generating output files or folders
- Installing packages or running scripts

**Example prompts:**

> "Create a Python script that loads regional_panel.csv, merges it with infrastructure_grants.csv on region_id and year, and saves the result to data/merged_panel.csv."

> "Edit the regression script to add year fixed effects and cluster standard errors at the region level."

> "Create a results/ folder with subfolders for tables/ and figures/."

**What happens:** Cursor proposes changes and shows you exactly what it will do. You review and approve each change before it is applied.

### Permissions

Agent Mode asks permission before taking actions:

- Creating or editing files
- Running terminal commands
- Installing packages

Always review proposed changes before accepting. Cursor may misunderstand your intent.

**Common Mistake:** Giving vague instructions. "Clean up my code" is unclear—Cursor may make unwanted changes. Be specific: "Add comments explaining what each section of the regression script does."

**Common Mistake:** Accepting changes without reading them. Always review what Cursor proposes before clicking Accept.

---

## 2.3 Agent Mode Security Settings

By default, Agent Mode asks for approval before running each terminal command. This is safe but can slow down your workflow when running routine commands like `python script.py` or `pip install pandas`.

Cursor provides settings to automate approval for trusted commands while maintaining safety.

### Sandboxed Terminals

On macOS, Cursor runs terminal commands in a **sandbox by default**. This means:
- Commands have read/write access to your project folder
- Commands cannot access the internet (unless you allow it)
- Commands cannot modify files outside your project

Keep sandboxing enabled. It provides protection if a command does something unexpected.

### Auto-Run Mode (YOLO Mode)

Auto-run mode allows specified commands to execute without manual approval each time.

**Location:** Cursor Settings → Features → enable "YOLO mode" (also called Auto-Run)

When enabled, you can configure:
- **Allowlist**: Commands that run automatically
- **Prompt**: Natural language description of what's allowed
- **File deletion protection**: Prevents accidental file deletion (keep this ON)

### Recommended Allowlist for Research Projects

These commands are safe to auto-approve for typical economics research work:

**Running Python scripts:**
```
python
python3
python -m
pytest
```

**Package management:**
```
pip install
pip list
pip freeze
pip show
```

**Virtual environment:**
```
source venv/bin/activate
venv/bin/activate
python -m venv
```

**File and folder operations:**
```
mkdir
touch
cp
mv
ls
pwd
cat
head
tail
wc
```

**Git operations:**
```
git status
git add
git commit
git diff
git log
git branch
git checkout
git pull
git push
git stash
```

**Jupyter:**
```
jupyter notebook
jupyter lab
```

### Example Auto-Run Prompt

Instead of listing individual commands, you can describe what's allowed:

> "Allow: running Python scripts, pip install, git commands, creating and viewing files (mkdir, touch, cat, head, ls), and Jupyter. Do not allow: rm, curl, wget, or any command that deletes files or accesses the internet."

### Commands to Keep Manual

Always review these before approving:
- `rm` (deleting files)
- `curl`, `wget` (downloading from internet)
- Any command you do not recognize
- Commands with `sudo`
- Commands modifying system files

### Recommended Approach

1. **When starting out**: Keep auto-run OFF. Review each command to learn what Cursor does.
2. **Once comfortable**: Enable auto-run with the allowlist above. Enable file deletion protection.
3. **Always**: Commit your work before running Agent Mode on significant tasks. If something goes wrong, you can revert.

---

## 2.4 Plan Mode

Use Plan Mode when starting a new project or planning a significant piece of analysis. Plan Mode uses Claude Opus 4.5 to think through the approach before writing any code.

**When to use:**
- Starting a new empirical project
- Planning a replication study
- Designing a series of robustness checks
- Structuring a complex data pipeline

**Example prompt:**

> "Plan the analysis for estimating the effect of infrastructure spending on regional employment. I have panel data for 50 regions from 2015-2023. Include data cleaning, baseline regression with fixed effects, and robustness checks."

**What happens:** Cursor generates a structured plan:

```
# Analysis Plan: Infrastructure and Employment

## Steps
1. Load and merge regional_panel.csv with infrastructure_grants.csv
2. Clean data: check for missing values, create lagged variables
3. Generate summary statistics and time series plots
4. Estimate baseline fixed-effects model (region and year effects)
5. Robustness checks: lagged spending, additional controls, subsample analysis
6. Export tables to LaTeX, figures to PNG

## Open Questions
- Include region-specific time trends?
- How to handle regions with zero grants in some years?
```

You can revise the plan by discussing it with Cursor. Once satisfied, execute it:

> "Proceed with step 1 of the plan."

Cursor switches to Agent Mode and implements each step, pausing for your approval.

---

## 2.5 Choosing the Right Mode

| What you want to do | Mode |
|---------------------|------|
| Understand what code does | Ask |
| Learn about a statistical method | Ask |
| Get help with syntax | Ask |
| Create a new script | Agent |
| Edit existing code | Agent |
| Run a command or install a package | Agent |
| Start a new project | Plan |
| Design a series of analyses | Plan |

**Rule of thumb:**
- **Ask** = learning and understanding
- **Agent** = doing and changing
- **Plan** = thinking and designing (then Agent to execute)

---

## 2.6 Providing Context to Cursor

Cursor can read files in your project, but you can also explicitly reference specific files, folders, or code in your prompts.

### Referencing Files with @

Type `@` in the chat to reference specific files:

> "Look at @data_cleaning.py and tell me what variables are created."

> "Compare the approach in @01_baseline.py with @02_robustness.py."

When you type `@`, Cursor shows a list of files in your project. Select the file you want to reference.

### Referencing Folders

Type `@` followed by a folder name to reference an entire folder:

> "Review the scripts in @code/ and explain the analysis pipeline."

### Referencing Code Selections

Select code in the editor, then open chat. The selected code is automatically included as context.

You can also use `@Code` to reference specific code snippets by name:

> "Explain what @run_regression does."

### Adding Files Without Explicit Reference

Use `#` to add files to the context without creating a formal reference. This is useful when you want Cursor to be aware of a file without explicitly discussing it.

### Referencing Documentation

Type `@` and select `Docs` from the dropdown to reference external documentation. You can also reference popular libraries directly:

> "@pandas - explain the different join types for DataFrame.merge"

To add custom documentation, use `@Docs` → `Add new doc` and provide the documentation URL.

### When to Use Explicit References

Use `@` references when:
- You want Cursor to focus on specific files rather than searching the project
- You are comparing multiple files
- You want to ensure Cursor sees the current state of a file

Cursor can also find files on its own based on your description. If you say "look at the regression script," Cursor will search for it. Explicit `@` references are more precise.

**Note:** Recent versions of Cursor have streamlined context gathering. In Agent Mode, Cursor automatically searches the codebase and gathers relevant context without requiring explicit `@` references. You can still use `@` when you want to be precise about which files to include.

---

## 2.7 Working with Chat Sessions

Within a single chat session, Cursor remembers your conversation. You can refer to previous context:

> "Now add robust standard errors to that regression."

Cursor understands "that regression" from the earlier discussion.

**Between sessions:** When you start a new chat, Cursor does not remember previous conversations. However, if you maintain a memory bank (see Chapter 6), Cursor can read your project documentation at the start of each session. The memory bank preserves project context, data decisions, and methodological choices across sessions.

**When to save manually:** The memory bank handles most persistent context. Occasionally, you may want to explicitly save something important—a particularly useful explanation or a key decision—by asking Cursor to update the relevant memory bank file:

> "Add this decision about handling missing values to the methods file in the memory bank."

**When to start fresh:** If Cursor seems confused or keeps referencing outdated information, start a new chat session using the "New Chat" button.

---

[← Previous: Getting Started](01-getting-started.md) | [Next: Effective Prompts →](03-effective-prompts.md)
