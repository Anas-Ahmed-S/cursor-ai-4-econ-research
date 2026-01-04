# Chapter 3: Effective Prompts

---

**What's in this chapter:**

| Section | Topic | Skip if... |
|---------|-------|------------|
| 3.1 | Conversations, not commands | You already use discussion-style prompts |
| 3.2 | Share your thinking | You want specific examples first |
| 3.3 | Ask Cursor to ask questions | You always know exactly what you need |
| 3.4 | Be specific when you know what you want | You prefer exploratory conversations |
| 3.5 | Specify output requirements | You do not have formatting needs |
| 3.6 | Common mistakes | Quick list of pitfalls to avoid |
| 3.7 | Example: A full discussion | You want to see a complete conversation |

---

The quality of Cursor's output depends on the quality of your input. This chapter explains how to communicate effectively with Cursor—not by memorizing prompt templates, but by having productive conversations.

---

## 3.1 Conversations, Not Commands

The most effective way to use Cursor is to have a discussion, not issue one-line commands.

### One-Line Prompts (Less Effective)

> "Analyze my data"

> "Run a regression"

> "Make a graph"

These prompts give Cursor no guidance. What analysis? Which variables? What kind of graph? Cursor will guess, and the guess may not match what you need.

### Discussion-Style Prompts (More Effective)

> "I'm trying to understand the relationship between infrastructure spending and employment in my panel data. I have 50 regions over 9 years. I think there might be regional differences that are fixed over time—like some regions are always more industrial than others—and I want to account for those.
>
> I'm not sure whether to use fixed effects or some other approach. Can you help me think through this and then run the appropriate analysis?"

This prompt shares:
- What you are trying to understand
- What data you have
- Your current thinking
- Your uncertainty
- An invitation to discuss before acting

Cursor can now ask clarifying questions, suggest approaches, and proceed with a shared understanding.

---

## 3.2 Share Your Thinking

When you explain your reasoning—even if uncertain—Cursor can engage with your ideas rather than starting from scratch.

**Without reasoning:**
> "Create a lag variable for infrastructure spending"

**With reasoning:**
> "I want to create a lag variable for infrastructure spending because the effect on employment probably isn't immediate—spending this year might affect employment next year. Does a one-year lag make sense, or should I try multiple lags?"

The second version invites Cursor to comment on your thinking. It might confirm the one-year lag is reasonable, suggest also trying two-year lags, or point out that with annual data you only have limited lag options.

---

## 3.3 Ask Cursor to Ask Questions

If you are unsure about something, say so and ask Cursor to help you figure it out.

**Example:**
> "I need to handle missing values in my infrastructure spending variable—about 5% are missing. I'm not sure whether to drop those observations, impute them, or just leave them and let the analysis handle it. What questions should I be thinking about here? Can you help me decide?"

Cursor might respond by asking:
- Are the missing values random, or concentrated in certain regions or years?
- How will the analysis use this variable—is it the main treatment or a control?
- What does missing mean here—no data, or zero spending recorded as missing?

This dialogue leads to a better decision than either dropping or imputing without thought.

---

## 3.4 Be Specific When You Know What You Want

Discussion is valuable when you are uncertain. When you know exactly what you need, be direct and specific.

**Vague:**
> "Load the data"

**Specific:**
> "Load data/regional_panel.csv into a pandas DataFrame. The file has columns region_id, year, employment_rate, education_rate, and gdp_per_capita. Set region_id and year as the index."

**Vague:**
> "Run a regression"

**Specific:**
> "Run a fixed-effects regression with employment_rate as the dependent variable and infrastructure_spending as the independent variable. Include region and year fixed effects. Cluster standard errors at the region level. Use linearmodels."

When you are specific, Cursor executes exactly what you request without guessing.

---

## 3.5 Specify Output Requirements

If you have requirements for how results should look, state them upfront.

**For tables:**
> "Export the regression results to LaTeX format. Include standard errors in parentheses below coefficients, significance stars at 10/5/1% levels, and the number of observations. Save to results/tables/baseline.tex."

**For figures:**
> "Save the figure as PNG at 300 DPI, suitable for journal submission. Use black and white only. Label axes clearly."

**For code:**
> "Add comments explaining the econometric reasoning, not just what the code does mechanically."

---

## 3.6 Common Mistakes

**Assuming Cursor remembers previous sessions.** Each new chat starts fresh. If you need context from earlier work, either reference your memory bank or re-explain.

**Combining unrelated requests.** "Load the data, run a regression, explain fixed effects, and tell me if my results are significant" is four different tasks. Ask one at a time, or break into numbered steps.

**Not specifying libraries.** If you say "run a panel regression," Cursor may choose any library. If you have a preference, state it: "Use linearmodels for the panel regression."

**Accepting output without checking.** Always verify that results make sense. Check coefficient magnitudes, sample sizes, and whether signs match your expectations.

---

## 3.7 Example: A Full Discussion

Here is how a discussion-style interaction might unfold:

**You:**
> I'm working on my infrastructure-employment analysis. I've run the baseline regression and the coefficient on infrastructure spending is 0.032 with a standard error of 0.008. Before I move on to robustness checks, I want to make sure I'm interpreting this correctly.
>
> Does this mean that for every additional unit of infrastructure spending, employment increases by 0.032 percentage points? That seems small. Or am I misunderstanding something? What should I check?

**Cursor:**
> The interpretation depends on the units of your variables. A few things to check:
>
> 1. What are the units of infrastructure_spending? If it's in millions of dollars, then a $1 million increase is associated with a 0.032 percentage point increase in employment—which is economically small. If it's in thousands, the interpretation changes.
>
> 2. What is the range of infrastructure_spending in your data? If spending ranges from 0 to 100 (millions), then going from no spending to average spending might have a meaningful cumulative effect.
>
> 3. Is employment_rate in percentage points (e.g., 65.0) or a proportion (e.g., 0.65)? This affects the magnitude.
>
> Want me to check the units and ranges in your data?

**You:**
> Yes, check the data. I think spending is in millions and employment is in percentage points like 65.0, but I want to confirm.

**Cursor:**
> Confirmed. infrastructure_spending ranges from 0 to 89 million, with a mean of 23 million. employment_rate ranges from 52.1 to 71.3 percentage points.
>
> So your coefficient means: a $1 million increase in infrastructure spending is associated with a 0.032 percentage point increase in employment. For a region going from the 25th percentile of spending ($8M) to the 75th percentile ($35M)—a $27M increase—the predicted employment effect is 0.86 percentage points.
>
> Whether that's economically meaningful depends on your context. In terms of statistical significance, with SE = 0.008, the coefficient is highly significant (t ≈ 4).

This exchange demonstrates the value of sharing your interpretation, expressing uncertainty, and asking Cursor to help you verify your understanding.

---

[← Previous: AI Modes](02-ai-modes.md) | [Next: Working with Data →](04-working-with-data.md)
