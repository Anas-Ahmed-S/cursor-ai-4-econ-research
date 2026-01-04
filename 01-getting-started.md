# Chapter 1: Getting Started

---

## Guide Overview

This guide consists of seven chapters covering Cursor from installation through practical research workflows.

| Chapter | Title | Content |
|---------|-------|---------|
| 1 | Getting Started | Installation, interface, Python setup |
| 2 | AI Modes | Ask, Agent, and Plan modes; context referencing; security settings |
| 3 | Effective Prompts | Conversational approach to working with AI |
| 4 | Working with Data | Data cleaning, visualization, regression workflows |
| 5 | Rules and Commands | User and project configuration; reusable prompts |
| 6 | Memory Banks | Persistent project documentation across sessions |
| 7 | Project Organization | Folder structure examples; version control; collaboration |

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 1.1 | About this guide | You want to jump straight to setup |
| 1.2 | What is Cursor | You already know what Cursor does |
| 1.3 | Installation | Cursor is already installed |
| 1.4 | Installing Python and libraries | Python is already set up |
| 1.5 | The Cursor interface | You are familiar with VS Code |
| 1.6 | Working with project folders | You understand project-based editors |
| 1.7 | The example dataset | You want to see the sample data used in this guide |
| 1.8 | Verifying your setup | Final checklist before proceeding |

---

## 1.1 About This Guide

This guide is written for economists, PhD students, and researchers who work primarily in Stata, R, or LaTeX and wish to learn how to use Cursor, an AI-assisted code editor.

No prior experience with AI coding tools is assumed. If you have used VS Code, some interface elements will be familiar. If you have not, the guide explains each component from scratch.

**Prerequisites:**
- Basic familiarity with any programming environment (Stata, R, or similar)
- A computer running macOS, Windows, or Linux
- An internet connection for downloading software and using AI features

**Conventions used in this guide:**

| Convention | Meaning |
|------------|---------|
| `monospace text` | Code, commands, file names, or variable names |
| > Block quote | Example prompts to type into Cursor |
| **Common Mistake** | A frequent error to avoid |

---

## 1.2 What is Cursor?

Cursor is a code editor with built-in AI assistance. It is built on the same foundation as Visual Studio Code (VS Code), so its interface and features will be recognizable to anyone who has used VS Code.

The difference between Cursor and a standard code editor is that Cursor can:

- Answer questions about your code and explain what it does
- Generate new code based on natural language instructions
- Edit existing code according to your specifications
- Read multiple files in your project to understand context
- Execute multi-step tasks involving file creation, modification, and terminal commands

**AI as Advisor, Not Replacement:**

Modern AI models (particularly Claude Opus 4.5) can provide substantive econometric advice. If you ask whether fixed effects or random effects are appropriate for your data structure, the AI can explain the assumptions, suggest diagnostic tests, and recommend specifications.

However, AI should not replace your econometric judgment. Use it as a colleague who can explain concepts, suggest approaches, and identify potential issues—but make final methodological decisions yourself. Understanding why you chose a specification matters for defending your work and catching errors.

**Recognizing Errors in AI-Generated Code:**

AI may produce code that runs without errors but produces incorrect results. This often happens when your prompt is ambiguous or incomplete—the AI interprets your request differently than you intended.

Develop the skill to recognize errors from output without reading every line of code:

- **Check magnitudes**: If your coefficient implies that a $1 million infrastructure grant increases employment by 50 percentage points, something is wrong.
- **Verify signs**: If the coefficient has an unexpected sign, investigate before assuming it is correct.
- **Compare to benchmarks**: If prior literature finds effects around 0.02 and your estimate is 2.0, check whether the calculation is correct.
- **Examine summary statistics**: If your "cleaned" data has 10 observations when you expected 450, a merge or filter went wrong.

Build verification checks into your workflow:

- Print intermediate outputs (row counts after merges, variable means after transformations)
- Compare results to hand calculations on a small subset
- Run sanity checks that should produce known results

When something looks wrong, then read the code carefully to find the error. But the first line of defense is recognizing implausible results.

---

## 1.3 Installation

### Downloading Cursor

1. Open a web browser and navigate to [cursor.com](https://cursor.com)
2. Click the download button for your operating system (macOS, Windows, or Linux)
3. Open the downloaded installer and follow the installation prompts
4. On macOS, drag Cursor to your Applications folder. On Windows, run the installer executable.

### Creating an Account

When you first launch Cursor, you will be prompted to create an account or sign in.

1. Click "Sign up" to create a new account
2. Enter your email address and create a password, or sign in with a Google or GitHub account
3. Verify your email if prompted

Cursor offers a free tier with limited AI usage and paid subscriptions with expanded features:

| Plan | Price | Usage |
|------|-------|-------|
| Hobby (Free) | $0 | 2,000 code completions, 50 slow requests |
| Pro | $20/month | Unlimited completions, ~225 Sonnet 4 requests |
| Pro Plus | $60/month | ~675 Sonnet 4 requests, higher limits |

Starting with the Pro plan ($20/month) is sufficient for most research work. The Pro Plus plan ($60/month) provides approximately three times the usage and may be appropriate for intensive use or longer sessions. The option to upgrade to Pro Plus appears in the app when usage limits are reached.

### First Launch Configuration

After signing in, Cursor may present configuration options:

- **Theme**: Choose light or dark mode based on preference. This does not affect functionality.
- **AI Model**: Select the default AI model. See model selection below.
- **Telemetry**: Decide whether to send usage data. This is optional.

### Model Selection

Cursor can use different AI models for different tasks.

| Task Type | Model | When to Use |
|-----------|-------|-------------|
| Planning and complex reasoning | Claude Opus 4.5 | Plan Mode, designing analysis strategies, methodological questions |
| Code generation and execution | Claude Sonnet 4.5 | Agent Mode, writing scripts, routine tasks |
| Quick questions | Claude Sonnet 4.5 | Ask Mode, syntax help, short explanations |

**Claude Opus 4.5** is suited for tasks requiring careful reasoning: planning a new project, deciding on econometric specifications, debugging complex issues, or working through methodological problems.

**Claude Sonnet 4.5** is suited for execution tasks: writing code according to a plan, making straightforward edits, or generating standard outputs.

You can change the model for each conversation in the chat panel settings.

**Note:** Model availability and names change over time as new versions are released. Check Cursor's model selector for current options.

---

## 1.4 Installing Python and Libraries

This guide uses Python with the following libraries:

- **pandas**: Data manipulation and analysis
- **statsmodels**: Statistical models and econometric methods
- **matplotlib**: Visualization and plotting
- **linearmodels**: Panel data estimation (fixed effects, random effects)

### Verifying Python Installation

Open the Cursor terminal (View → Terminal, or press `` Ctrl+` `` on Windows/Linux, `` Cmd+` `` on macOS) and type:

```bash
python --version
```

or on some systems:

```bash
python3 --version
```

You should see output like `Python 3.10.12` or similar. If Python is not installed, download it from [python.org](https://python.org).

### Setting Up a Virtual Environment

Create a separate virtual environment for each research project. This keeps dependencies isolated and prevents conflicts between projects.

**Using Cursor to set up the environment:**

In Agent Mode, prompt:

> "Create a Python virtual environment called 'venv' in this project folder. Activate it and install pandas, statsmodels, matplotlib, and linearmodels."

Cursor will execute the following (or equivalent) commands:

```bash
python -m venv venv
source venv/bin/activate    # On macOS/Linux
# or: venv\Scripts\activate  # On Windows
pip install pandas statsmodels matplotlib linearmodels
```

After setup, Cursor should automatically detect and use the virtual environment. You can verify by checking that the terminal prompt shows `(venv)` or by running:

```bash
which python    # Should point to venv/bin/python
```

### Installing Additional Libraries

When you need additional packages during your project, use Agent Mode:

> "Install the scipy library in the current virtual environment."

Cursor will run `pip install scipy` in the correct environment.

**Common Mistake:** Multiple Python installations can cause confusion. If you have both a system Python and an Anaconda installation, Cursor may select the wrong one. Using virtual environments (as described above) avoids this problem by creating a project-specific Python environment.

---

## 1.5 The Cursor Interface

Cursor's interface consists of several components:

### Main Editor Pane

The central area where files are displayed and edited. You can have multiple files open in tabs.

### Sidebar

Located on the left side of the window. Contains:

- **Explorer**: Browse files and folders in your project
- **Search**: Find text across all files
- **Source Control**: Git integration for version control
- **Extensions**: Install additional functionality

### Bottom Panel

Located below the main editor. Contains:

- **Terminal**: A command-line interface for running scripts and installing packages
- **Output**: Displays output from running processes
- **Problems**: Lists errors and warnings in your code

### Chat Panel

The AI interaction panel, accessible from the sidebar or by pressing `Cmd+L` (macOS) or `Ctrl+L` (Windows/Linux). This is where you type prompts and receive AI responses.

The chat panel can appear:
- As a sidebar panel alongside the file explorer
- As an inline popup within the editor

---

## 1.6 Working with Project Folders

Cursor works best when you open an entire project folder rather than individual files.

**To open a project folder:**

1. Go to File → Open Folder
2. Navigate to your project directory and select it
3. Click Open

When you open a folder, Cursor can:
- See all files in the project
- Understand relationships between scripts
- Provide context-aware suggestions based on your data files and analysis code

**Common Mistake:** Opening files individually (File → Open File) instead of opening the project folder limits Cursor's ability to understand context. If you open only `regression.py`, Cursor cannot see `data_cleaning.py` or your data files, and its suggestions will be less accurate.

**Common Mistake:** Renaming or moving your project folder causes Cursor to lose chat history. Cursor stores conversation history based on the folder path. If you rename `infrastructure_study/` to `employment_analysis/`, or move the folder to a different location, previous chat conversations will no longer appear when you open the project. The conversations still exist but are associated with the old path. Decide on folder names and locations before starting significant work.

---

## 1.7 The Example Dataset

Throughout this guide, examples use a fictional dataset on regional employment and infrastructure spending.

**Dataset Description:**

| File | Description |
|------|-------------|
| `regional_panel.csv` | Panel data for 50 regions, years 2015–2023 |
| `infrastructure_grants.csv` | Federal infrastructure grant amounts by region and year |

**Variables in `regional_panel.csv`:**

| Variable | Description |
|----------|-------------|
| `region_id` | Unique identifier for each region (1–50) |
| `year` | Year of observation (2015–2023) |
| `employment_rate` | Employment-to-population ratio (percent) |
| `education_rate` | Share of population with post-secondary education (percent) |
| `gdp_per_capita` | GDP per capita in constant dollars |
| `population` | Total population |

**Variables in `infrastructure_grants.csv`:**

| Variable | Description |
|----------|-------------|
| `region_id` | Unique identifier for each region |
| `year` | Year of grant |
| `infrastructure_spending` | Federal infrastructure grant amount (millions of dollars) |

**Research Question:**

What is the effect of federal infrastructure spending on regional employment rates, controlling for education and economic conditions?

This dataset will be used in later chapters to demonstrate data cleaning, merging, visualization, and regression analysis.

---

## 1.8 Verifying Your Setup

Before proceeding to the next chapter, confirm the following:

1. Cursor is installed and launches successfully
2. You can open the terminal within Cursor
3. Running `python --version` displays a Python version (3.8 or higher recommended)
4. Running `pip show pandas` displays package information (confirming pandas is installed)

If any of these steps fail, revisit the relevant sections above.

---

[Next: AI Modes →](02-ai-modes.md)
