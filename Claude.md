# primer.tutorials

This is an R package containing learnr tutorials that teach Bayesian data science using the Cardinal Virtues framework. The primary task is writing and editing tutorial `.Rmd` files.

For a full worked example, see `template_tutorial.Rmd`.

DO NOT edit:
- /renv
- renv.lock

---

## Tutorial Structure

Every tutorial follows this section order:
**Introduction → Wisdom → Justice → Courage → Temperance → Summary**

Each section begins with `## SectionName` followed by `### ` (an empty subsection header) and a quote. Exercises are numbered within each section using chunk labels like `{r wisdom-3}`.

---

## The XX Placeholder Convention

- `XX` marks every location that requires editing. A finished tutorial has zero `XX` markers remaining.
- `[XX: unit]` means replace the entire bracketed expression with the right word (e.g., `[XX: unit]` → `candidate`).
- `fit_XX` → replace with your fitted object name, always prefixed with `fit_` (e.g., `fit_lm`).
- `XX.qmd` → the student's Quarto file name.
- After inserting questions, run `tutorial.helpers::check_current_tutorial()` to renumber exercises correctly.
- Delete all instruction comments (`<!-- XX: ... -->`) as you go. No instructions should remain in a finished tutorial.

---

## Question Types

There are three types of exercises:

**1. Code exercises** — students run R code interactively:
```r
{r section-N, exercise = TRUE}
# blank or starter code

{r section-N-hint-1, eval = FALSE}
# partial solution

{r section-N-test, include = FALSE}
# solution for testing
```
Include a "Copy previous code" button when building a pipeline step-by-step:
```html
<button onclick = "transfer_code(this)">Copy previous code</button>
```

**2. Written with answer** — uses `message =` and `allow_retry = FALSE`:
```r
question_text(NULL,
    message = "The correct answer goes here.",
    answer(NULL, correct = TRUE),
    allow_retry = FALSE,
    incorrect = NULL,
    rows = 6)
```

**3. Written without answer (CP/CR)** — student pastes console output; uses `allow_retry = TRUE`:
```r
question_text(NULL,
    answer(NULL, correct = TRUE),
    allow_retry = TRUE,
    try_again_button = "Edit Answer",
    incorrect = NULL,
    rows = 3)
```
CP/CR = the student copies output from the Console and pastes it into the answer box.

**Key rules for questions:**
- Never have students do an assignment themselves when creating an object for later use. Build the pipeline step-by-step, then say: *"Behind the scenes, we have assigned the result to `fit_obj`. To confirm, type `fit_obj` and hit Run Code."*
- Always provide an excellent written answer in `message =`. Students read these carefully.
- Causal/predictive questions: never use words like "cause," "impact," or "influence" for non-treatment variables. Use "when comparing" or "differences between groups."

---

## Setup Chunk Rules

```r
# Always required (not loaded by students):
library(learnr)
library(tutorial.helpers)
library(gt)

# Loaded by students during the tutorial:
library(tidyverse)
library(tidymodels)   # or another modeling package if tidymodels isn't used
library(broom)        # or broom.mixed
library(marginaleffects)
library(easystats)
```

- Data source packages (e.g., `library(primer.data)`) go after `library(tidyverse)`.
- Only include `library(tidymodels)` if it is actually used.
- Never include setup code that takes more than a few seconds to run. For slow operations, write/load the object — see `tutorial.helpers` instructions.
- The fitted model object lives in setup as `fit_XX` (replace XX). Always starts with `fit_`.

---

## Inline Plots (Knowledge Drops)

Plots shown to students without code use a bare, unlabeled chunk:
```r
```{r}
# plot code here
```
```
- Always include: title, subtitle (the main takeaway), axis labels.
- No code chunk label. Don't show code.

---

## Section-Specific Rules

### Introduction
- Start with an "Imagine that you are..." paragraph. End it with "There are many decisions to make."
- Covers: Cardinal Virtues review, GitHub repo setup, loading libraries, exploring data, framing causal vs. predictive model, Rubin Causal Model concepts.

### Wisdom
- Opens with a quote (choose one from options in template).
- Builds a Preceptor Table iteratively.
- If data needs cleaning, build a pipeline line-by-line (each step produces output), then assign the result to `x`.
- Ends with a specific Quantity of Interest question.
- Students write the first two sentences of a summary paragraph: (1) general statement about the topic, (2) introduces data source and specific question.

### Justice
- Covers: validity, stability, representativeness, unconfoundedness.
- Validity answers must use the word "column(s)."
- Delete the unconfoundedness question for non-causal models.
- Model structure is determined by outcome type:
  - Continuous → Normal / linear: `linear_reg(engine = "lm")`
  - Binary → Bernoulli / logistic: `logistic_reg(engine = "glm")`
  - Categorical → Multinomial: `multinom_reg(engine = "glmnet")`
  - Ordinal → Cumulative Logistic
- Show LaTeX for the mathematical structure (using generic $Y$, $X_1$, etc.).
- Students write a "weakness" sentence for the summary paragraph.

### Courage
- Fit model progressively: first a single categorical variable, then multi-level, then the final version.
- Final fitted object is `fit_XX`; it lives in the setup chunk.
- Run `check_predictions(extract_fit_engine(fit_XX))` in the Console (using `easystats`).
- Show LaTeX for the fitted model (estimates plugged in, hat on $\hat{Y}$, no error term).
- Model fitting chunk gets `#| cache: true`. Add `*_cache` to `.gitignore`.
- Create a `tidy(conf.int = TRUE)` table of parameters.
- Students write a sentence describing model structure for the summary paragraph.

### Temperance
- Interpret ~3 parameters using `tidy()` output shown at the start of each question.
- Use `marginaleffects`: `predictions()` first, then multiple `plot_predictions()` calls.
- Final plot: title, subtitle (key takeaway), caption (data source), clean axis labels.
- Use `draw = FALSE` to get a tibble for custom ggplot work.
- Students write a QoI + uncertainty sentence and a "why estimates might be wrong" sentence.
- Final exercise: rearrange QMD so graphic comes before paragraph, then publish to GitHub Pages.

### Summary
- Final plot + complete summary paragraph (general statement → data intro → weakness → model structure → QoI with uncertainty).

---

## Summary Paragraph Structure

The paragraph built across the tutorial has this shape:
1. **(Wisdom)** General statement + data source + specific question.
2. **(Justice)** One weakness/assumption concern.
3. **(Courage)** Model structure sentence.
4. **(Temperance)** Quantity of interest with uncertainty. Why estimates might be wrong.
