# CLAUDE.md

This file is the working reference for writing chapters of the textbook *Preceptor's Primer for Bayesian Data Science: Using the Cardinal Virtues for Inference* and the matching learnr tutorials in the `primer.tutorials` package. It is addressed to Claude. David Kane is the author; Claude is the co-author he collaborates with to produce new material.

The goal is that this file is the only reference either of us needs when starting work on a new chapter/tutorial pair. If any other document in the project conflicts with what's written here, this file wins.

---

## 1. Project

The *Primer* teaches students how to do data science: given a question and some data, complete a series of steps that, with luck, yield an answer — presented graphically, and including a measure of uncertainty.

The steps are organized around the [Cardinal Virtues](https://en.wikipedia.org/wiki/Cardinal_virtues) — Wisdom, Justice, Courage, Temperance. References and allusions to the virtues are a feature, not a bug. Use them liberally.

The project has three artifacts:

- **Textbook chapters** — full Quarto files, one per chapter, numbered. The audience reads these but fewer than a quarter of students actually will.
- **Tutorials** — one learnr tutorial per chapter, in the `primer.tutorials` package. Students *are* required to complete these, so they must be self-contained; do not assume a student has read the chapter.
- **Classroom material** — not yet created.

### 1.1 Two types of chapters/tutorials

**Example** chapters/tutorials work through a well-defined data science problem using the Cardinal Virtues. The example sequence gets progressively more sophisticated as students become more practiced. Every example chapter covers every important concept — causal effect, Preceptor Table, Population Table, hypothesis testing, posterior predictive checks, and so on. Earlier example chapters may skip the most advanced concepts, but once a concept is introduced it appears in every subsequent chapter.

**Miscellaneous** chapters/tutorials cover topics that do not involve a major data science exercise. The current five are Probability, Sampling, Rubin Causal Model, Cardinal Virtues, and Mechanics.

### 1.2 Chapter ≠ tutorial

Chapters are longer than tutorials. Every example chapter uses the same dataset to build **both a predictive model and a causal model** — two Preceptor Tables, two Population Tables, two final answers. The matching tutorial only has time for one model, and that model is also covered (in more detail) in the chapter. The other model in the chapter is extra material not in the tutorial.

Almost every line of code executed in the tutorial is also executed in the chapter, but the chapter does more: more EDA, more candidate models, more exploration.

The "Imagine that you are…" opener is the same in the chapter and the tutorial. Reuse, don't rewrite.

---

## 2. Working with David

The authoring of a chapter/tutorial pair is a conversation. Do not try to produce a finished chapter in one shot. The rough protocol:

1. **David picks the topic or gives a pointer.** Example: "Write chapter 9, on logistic regression."
2. **Claude proposes the framing.** Candidate dataset(s), the Imagine-that-you-are scenario, the broad question, a specific narrow question (the QoI), and whether the tutorial will use a predictive or causal model. Offer two or three options where there is real choice.
3. **David picks.** Iterate on the dataset, the unit, the outcome, the treatment (if causal), and a short list of covariates.
4. **Claude drafts the Preceptor Table and Population Table** as `gt` code. David reviews and corrects.
5. **Claude drafts the chapter Wisdom section**, then the tutorial Wisdom section. David reviews. Repeat by virtue: Justice, Courage, Temperance.
6. **Claude checks spaced-repetition coverage** against the tutorial index in §15 and adjusts which recurring questions this tutorial asks.

This protocol is a default; deviate when it makes sense. Where a decision is small and reversible (phrasing of a knowledge drop, which concrete example to use in an exercise), just make it. Where a decision shapes the rest of the chapter (dataset, QoI, functional form), pause and ask.

When you pause to ask, make it easy for David to answer: short list of options, your recommendation, your reasoning. Do not ask open-ended questions when a multiple-choice question will do.

---

## 3. Output artifacts and file conventions

Chapters are Quarto files with the top-level `#` already set by the book structure; use `##` for virtue-level sections. Filename convention: `NN-topic-name.qmd` where `NN` is the chapter number (e.g. `04-cardinal-virtues.qmd`, `09-logistic-regression.qmd`).

Tutorials are R Markdown files with `learnr::tutorial` output. They live in the `primer.tutorials` package under `inst/tutorials/NN-topic-name/tutorial.Rmd` (or similar — confirm with David before writing the first one in a new package state). The tutorial's `id` in the YAML is `NN-topic-name`, lowercase, dashes for spaces.

For new chapters, produce a single `.qmd` file. For new tutorials, produce a single `.Rmd` file with the structure described in §5. Do not emit partial diffs; produce complete files David can drop in place.

---

## 4. Chapter structure

Every example chapter has six top-level sections under `#`:

1. **Introduction** — `##`-level. Names the four Cardinal Virtues. Gives one "Imagine that you are…" paragraph motivating the problem. Names the dataset. Typically 2–6 paragraphs.
2. **Wisdom** — question, Preceptor Table (predictive), EDA, Preceptor Table (causal).
3. **Justice** — Population Table, validity, stability, representativeness, unconfoundedness (unconfoundedness applies only to causal models).
4. **Courage** — mathematical structure, candidate models, tests, the selected Data Generating Mechanism. In later chapters, a posterior predictive check.
5. **Temperance** — interpretation, questions and answers, humility.
6. **Summary** — one final graphic, one concluding paragraph, and the sentence "The world is always more uncertain than our models would have us believe."

Chapters include full image references (`knitr::include_graphics("other/images/Wisdom.jpg")` etc.) at the top of each virtue section. Chapters quote extensively — from Tukey, from Rumsfeld, from the Bible, from whomever fits. Quotes are good; use them.

Chapters use the predictive model first and the causal model second where both apply. Both models share the dataset but not the question.

---

## 5. Tutorial structure

Every example tutorial has the same six sections as a chapter (Introduction, Wisdom, Justice, Courage, Temperance, Summary), but each section is a sequence of `### Exercise N` blocks rather than prose.

Tutorials are self-contained. A student who has not read the chapter should be able to complete the tutorial and learn everything the tutorial is trying to teach. This means every tutorial defines the key terms (causal effect, Preceptor Table, Population Table, validity, stability, representativeness, unconfoundedness, DGM), even though the chapter does too.

Every tutorial fits **one model**, not two — either predictive or causal, matching whichever the chapter spends more time on.

### 5.1 YAML header

```yaml
---
title: <Topic>
author: David Kane
tutorial:
  id: <NN-topic-name>   # e.g., 09-logistic-regression. Same as directory name.
output:
  learnr::tutorial:
    progressive: yes
    allow_skip: yes
runtime: shiny_prerendered
description: "Tutorial #NN for Preceptor's Primer"
---
```

### 5.2 Setup chunk

The setup chunk loads the packages needed to render the tutorial and fits the model that the tutorial will reference as `fit_<n>` (e.g. `fit_attitude`). It must not contain anything slow; setup runs every time the tutorial launches.

```r
```{r setup, include = FALSE}
# learnr, tutorial.helpers, and gt are required for the tutorial to work.
# gt is needed because we show a Preceptor Table built with it, even
# though we don't show students how to build it.

library(learnr)
library(tutorial.helpers)
library(gt)

# Packages below are what we want students to load themselves in the
# Console/QMD. They are listed in the tutorial so that (a) we have access
# to their functions for rendering, and (b) students get a knowledge drop
# for each.

library(tidyverse)
library(tidymodels)       # or ordinal, or another modeling package
library(broom)            # or broom.mixed
library(marginaleffects)
library(easystats)        # used for check_predictions()

knitr::opts_chunk$set(echo = FALSE)
options(tutorial.exercise.timelimit = 600,
        tutorial.storage = "local")

# Fit the model used throughout the tutorial. Name it fit_<something>.
# Keep setup cheap — a few seconds at most.
fit_<n> <- linear_reg(engine = "lm") |>
  fit(<outcome> ~ <covariates>, data = <tibble>)
```
```

Model naming: always start with `fit_` (`fit_attitude`, `fit_lifespan`, etc.) and use the same name throughout the tutorial.

Slow setup is a hard no. See the `tutorial.helpers` AI article (https://ppbds.github.io/tutorial.helpers/articles/ai.html) for background.

### 5.3 Child documents

Three child-document inclusions are standard. Place them immediately after the setup chunk, before the Introduction section:

```r
```{r copy-code-chunk, child = system.file("child_documents/copy_button.Rmd", package = "tutorial.helpers")}
```

```{r info-section, child = system.file("child_documents/info_section.Rmd", package = "tutorial.helpers")}
```
```

The third child document, `download_answers.Rmd`, goes at the very end of the tutorial (after Summary):

```r
```{r download-answers, child = system.file("child_documents/download_answers.Rmd", package = "tutorial.helpers")}
```
```

These are part of the framework. Do not reinvent them.

### 5.4 Sections and numbering

Each of the six sections opens with `##`. Each exercise opens with `### Exercise N` where `N` restarts at 1 within each section. Each exercise has a chunk label of the form `<section>-<N>` — `introduction-1`, `wisdom-3`, `justice-11`, etc.

---

## 6. Question flow

Within a section, each `### Exercise N` has three parts: **Start**, **exercise code chunk(s)**, and **End**. Every exercise ends with at least one Continue button (triple hash, `###`) before the next one begins.

### 6.1 Start

The Start is one or two sentences of framing and then the question itself. Two rules:

- **Two-sentence rule.** Students will not read more than two sentences at a time. If the Start is longer than two sentences, insert a `###` (Continue button) to break it into pieces.
- If the Start is short (one or two sentences), the question code chunk follows immediately without a `###` between them.

Students tend to click Continue until they see a question. They then read the sentence or two *immediately before* the question closely, because they don't know whether that text is needed to answer. That is your best place to teach.

### 6.2 Exercise code chunk(s)

Follow the `tutorial.helpers` conventions:

- **Exercise chunk.** Label it `{section}-{N}` (e.g., `wisdom-3`). This is where the student's answer goes.
- **Hint chunk.** Label `{section}-{N}-hint-1`. Include `eval = FALSE` because the hint is often not legal R code. Almost always only one hint.
- **Test chunk.** Label `{section}-{N}-test`. Include `include = FALSE`. Contains the code that the exercise expects to work — the canonical answer. We never show this to students; it exists so we can verify our own examples still run.

Hint and test chunks are only for code exercises. Written-answer exercises don't have them.

Most of the time there is **no** `###` immediately before the exercise code chunk — the Start runs directly into the code.

### 6.3 End

After the exercise code chunks, always place a `###` (to give the student a Continue button to pause on their output) and then a short End: one or two sentences of knowledge drop.

### 6.4 Topic-level final knowledge drop

After the last `### Exercise N` of a virtue section, add a standalone `###` followed by a one-or-two-sentence knowledge drop that steps back from the specific exercise and makes a broader point about the topic. This is not another exercise. It is the capstone for the section.

Example: if the last exercise in Wisdom polishes a scatter plot, the topic-final knowledge drop says something about scatter plots in general, or about the role of EDA, not about the particular plot the student just made.

---

## 7. Exercise types

Three types, used in roughly this mix:

### 7.1 Code exercise

Student writes R code in the exercise chunk. Has a hint chunk and a test chunk. The test chunk contains the canonical answer; the exercise chunk is empty (or has scaffolding). With AI-mediated authoring (§9), most code exercises are now single-shot: state the goal, let the student prompt AI, paste and run.

```r
```{r courage-3, exercise = TRUE}

```

```{r courage-3-hint-1, eval = FALSE}
linear_reg(engine = "lm") |>
  fit(...)
```

```{r courage-3-test, include = FALSE}
linear_reg(engine = "lm") |>
  fit(att_end ~ treatment, data = trains)
```
```

### 7.2 Written exercise with model answer

A `question_text()` with `message = "..."` containing the canonical answer, `allow_retry = FALSE`, `incorrect = NULL`. Used for questions that have a correct answer — definitions, conceptual framings, recall. Students see our answer after submitting theirs.

```r
```{r wisdom-2}
question_text(NULL,
    message = "A Preceptor Table is the smallest possible table of data with rows and columns such that, if there is no missing data, we can easily calculate the quantity of interest.",
    answer(NULL, correct = TRUE),
    allow_retry = FALSE,
    incorrect = NULL,
    rows = 6)
```
```

The text in `message` is read closely by students, who compare their answer to ours. Our answer must be excellent. Use the wording in §11 verbatim for definitional questions.

### 7.3 Written exercise without model answer

A `question_text()` with no `message`, `allow_retry = TRUE`, `try_again_button = "Edit Answer"`. Used only when there is no single correct answer — typically when the student is asked to run a diagnostic command like `show_file()` and paste the output, or to describe something specific to their own analysis. Do not use this type for definitional or conceptual questions.

```r
```{r introduction-2}
question_text(NULL,
    answer(NULL, correct = TRUE),
    allow_retry = TRUE,
    try_again_button = "Edit Answer",
    incorrect = NULL,
    rows = 3)
```
```

### 7.4 Operational conventions: "CP/CR" and `show_file()`

Many operational exercises end with the string **CP/CR**, short for *Copy-Paste / Command-Response*. Students know what it means by the time they get past the first tutorial. Exception: the **very first tutorial** should spell it out once, inside the exercise Start, before using it as shorthand.

`show_file()` (from `tutorial.helpers`) prints the contents of a file in the student's project. The usual pattern: the student does something in their QMD, then runs `show_file("XX.qmd", chunk = "Last")` in the Console to display the last chunk, copies the Console output, and pastes it back into the tutorial. `chunk = "Last"` is preferred over `start = -N` because it's more robust. We never actually check what they paste; the threat of checking is the point.

The `Cmd/Ctrl + Shift + K` keystroke renders the QMD. Use it often — rendering catches bugs early, and professionals do it.

---

## 8. Spaced repetition

We believe spaced repetition works, and we care more about repetition than precise spacing. The goal: three months after finishing the tutorials, a student can still answer questions about our key definitions.

**Example chapters** repeat all definitions and concepts every time — each chapter is self-contained.

**Example tutorials** do not repeat everything every time; doing so would make each tutorial too long. Instead, use a schedule like: ask in tutorials 1, 2, 3, skip tutorial 4, ask in 5, skip 6 and 7, ask in 8, skip 9 and 10, ask in 11, and so on. Exact cadence can vary; the principle is "ask often early, then space out."

Before writing a tutorial, read §15 to see which recurring questions are due this tutorial.

---

## 9. AI-mediated code exercises

Students use AI (ChatGPT, Claude, etc.) to produce code. They prompt the AI and paste the result into their tutorial. Design code exercises accordingly:

- **Do not build pipelines line-by-line with the student writing each line** the way older tutorials do. Instead, state the goal of the code clearly enough that a student could prompt an AI for it, then have them run the AI's output.
- **Be explicit about what the code should do, not what it should look like.** "Create a tibble called `x` that contains only the 1992 observations from `nes`, with no missing values" is better than a fill-in-the-blanks pipeline.
- **Knowledge drops still matter.** The student may not read the code the AI wrote carefully. Use the End of each exercise to point out what the code actually did and why it matters.

This affects the shape of Wisdom and Courage sections in particular. Wisdom has lots of "examine the data" exercises; those still make sense as AI-prompted code. Courage fits models; fitting is typically one-shot, so most of Courage is interpretation, not pipeline-building.

---

## 10. Preceptor and Population Tables

Every Preceptor Table and Population Table in the book and the tutorials follows the same format. Write them directly as `gt` code using the templates in §10.3 and §10.4: copy the template, change the column labels, fill in the example rows, and write the footnotes.

### 10.1 Purpose

- The **Preceptor Table** is the smallest possible table with rows and columns such that, if there is no missing data, the quantity of interest is easy to calculate. In causal tables, some cells show `"?"` — the unobserved potential outcome that the fundamental problem of causal inference forbids us from seeing.
- The **Population Table** combines observed data (from our dataset) and researcher expectations (from the Preceptor Table), with separator rows representing the broader population from which both are drawn. Each row is a unique unit/time combination.

### 10.2 Shared conventions

These apply to both tables.

- **All cell values are in double quotes, including numbers.** `"42"`, not `42`.
- **Labels are display phrases, not variable names.** `"Lifespan if Win"`, not `lifespan_win`. Capitalized, space-separated, human-readable.
- **Placeholder cells use `"..."`** — for blank rows, for the `More` column, and anywhere else we're gesturing at content we're not showing.
- **Unobserved potential outcomes use `"?"`** — only in causal tables, only in potential-outcome columns, only on data rows where the counterfactual is unavailable. Never `"..."` for this.
- **Column alignment:** center all columns, then left-align the first column (the unit label). The time column stays centered.
- **Column widths are flexible.** Set them with `gt::cols_width()` if the default looks cramped or sprawling; otherwise omit. A reasonable ballpark when you do set them: 80px for `Source`, 100–120px for unit/time/outcome/treatment/covariate columns, 60px for `More`.
- **`gt::fmt_markdown(columns = gt::everything())`** — so cell contents can contain emphasis, links, etc.
- **`gt::cols_label(More = "...")`** — so the `More` column header displays as `...` rather than the literal word.
- **Spanner IDs are fixed:** `"unit_span"`, `"outcome_span"`, `"treatment_span"`, `"covariates_span"`. Footnotes attach to these IDs, so don't rename them.

### 10.3 Preceptor Table

**Structure.** Four rows. Columns grouped under spanners:

- **Unit/Time** — two columns: a unit label (e.g., `Candidate`, `Senator`, `Student`) and a time label (e.g., `Election Year`, `Session Year`).
- **Potential Outcomes** (causal) or **Outcome** (predictive) — two or more columns for causal, one for predictive. Causal column names name the counterfactual: `Lifespan if Win`, `Lifespan if Lose`.
- **Treatment** (causal only) — one column.
- **Covariates** — one or more columns, plus a final `More` column that is always `"..."`.

Row 3 is blank (all `"..."`). Rows 1, 2, and 4 are concrete example entries.

**Footnotes.** Five, attached to the title and the four spanners via `gt::tab_footnote()`:

- *Title footnote* — states the question the table helps answer.
- *Unit footnote* — defines what each row represents. Connects to stability and representativeness.
- *Outcome footnote* — for causal, connects to validity and explains what the potential outcomes mean; for predictive, describes the outcome variable and its measurement.
- *Treatment footnote* — defines the treatment and connects to unconfoundedness. Omit for predictive.
- *Covariates footnote* — explains the covariate set and what the `More` column represents.

**Causal template.** Copy this and edit the labels, example rows, and footnote strings. (Example uses the `governors` / lifespan-and-elections problem.)

```r
p_tibble <- tibble::tribble(
  ~`Candidate`   , ~`Election Year`, ~`Lifespan if Win`, ~`Lifespan if Lose`, ~`Election Outcome`, ~`Election Age`, ~More ,
  "John Smith"   , "1975"          , "78"              , "75"               , "Won"              , "52"           , "...",
  "Mary Johnson" , "1982"          , "82"              , "79"               , "Lost"             , "48"           , "...",
  "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "...",
  "Robert Wilson", "1990"          , "75"              , "81"               , "Won"              , "45"           , "..."
)

pre_title_footnote      <- "The question we are trying to answer goes here."
pre_units_footnote      <- "Each row represents [unit] in [time period]. Missing rows represent the broader population."
pre_outcome_footnote    <- "Potential lifespans under winning vs. losing. Question marks show unobserved counterfactuals (validity)."
pre_treatment_footnote  <- "Election outcome from vote margin. Close races approximate random assignment (unconfoundedness)."
pre_covariates_footnote <- "Election age is the observed covariate. The 'More' column represents other variables we might consider."

gt::gt(p_tibble) |>
  gt::tab_header(title = "Preceptor Table") |>
  gt::tab_spanner(label = "Unit/Time"         , id = "unit_span",
                  columns = c(`Candidate`, `Election Year`)) |>
  gt::tab_spanner(label = "Potential Outcomes", id = "outcome_span",
                  columns = c(`Lifespan if Win`, `Lifespan if Lose`)) |>
  gt::tab_spanner(label = "Treatment"         , id = "treatment_span",
                  columns = c(`Election Outcome`)) |>
  gt::tab_spanner(label = "Covariates"        , id = "covariates_span",
                  columns = c(`Election Age`, More)) |>
  gt::cols_align(align = "center", columns = gt::everything()) |>
  gt::cols_align(align = "left"  , columns = c(`Candidate`)) |>
  gt::cols_label(More = "...") |>
  gt::fmt_markdown(columns = gt::everything()) |>
  gt::tab_footnote(footnote = pre_title_footnote,
                   locations = gt::cells_title()) |>
  gt::tab_footnote(footnote = pre_units_footnote,
                   locations = gt::cells_column_spanners(spanners = "unit_span")) |>
  gt::tab_footnote(footnote = pre_outcome_footnote,
                   locations = gt::cells_column_spanners(spanners = "outcome_span")) |>
  gt::tab_footnote(footnote = pre_treatment_footnote,
                   locations = gt::cells_column_spanners(spanners = "treatment_span")) |>
  gt::tab_footnote(footnote = pre_covariates_footnote,
                   locations = gt::cells_column_spanners(spanners = "covariates_span"))
```

**Predictive variant.** Three changes from the causal template:

1. The outcome spanner holds a single column and is re-labeled `"Outcome"`:
   ```r
   gt::tab_spanner(label = "Outcome", id = "outcome_span", columns = c(`<OutcomeColumn>`)) |>
   ```
2. Drop the `Treatment` spanner and the `tab_footnote` call for `pre_treatment_footnote`.
3. The outcome footnote describes the outcome variable and its measurement — no validity framing about counterfactuals needed.

### 10.4 Population Table

**Structure.** Eleven rows. Same column structure as the Preceptor Table, plus a leading `Source` column (not under any spanner) with values `"Data"`, `"Preceptor"`, or `"..."`.

Two kinds of blank row — this is subtle but matters:

- **Separator blanks** — rows 1, 6, and 11. *Every* cell is `"..."`, *including* `Source`. These represent the rest of the population.
- **Middle-of-group blanks** — rows 4 (middle of the data block) and 9 (middle of the preceptor block). Content cells are `"..."`, but `Source` keeps its value (`"Data"` in row 4, `"Preceptor"` in row 9). These represent "more of the same kind of row."

Full layout:

| Row | Source      | Content              |
|-----|-------------|----------------------|
| 1   | `"..."`     | separator, all `"..."` |
| 2   | `"Data"`    | observed data row    |
| 3   | `"Data"`    | observed data row    |
| 4   | `"Data"`    | middle blank, content cells `"..."` |
| 5   | `"Data"`    | observed data row    |
| 6   | `"..."`     | separator, all `"..."` |
| 7   | `"Preceptor"` | Preceptor Table row |
| 8   | `"Preceptor"` | Preceptor Table row |
| 9   | `"Preceptor"` | middle blank, content cells `"..."` |
| 10  | `"Preceptor"` | Preceptor Table row |
| 11  | `"..."`     | separator, all `"..."` |

The `More` column is present and is always `"..."` in every row.

**Footnotes.** Same five-footnote structure as the Preceptor Table, but the content leans toward data provenance:

- *Title footnote* — describes how this table combines observed data with Preceptor Table expectations.
- *Unit footnote* — distinguishes Data rows from Preceptor rows; connects to stability and representativeness.
- *Outcome footnote* — documents data sources and measurement procedures. For causal, connects to validity by explaining how observed outcomes relate to potential outcomes.
- *Treatment footnote* — explains how treatment was assigned or observed (unconfoundedness). Omit for predictive.
- *Covariates footnote* — describes covariate data sources and any measurement differences between the data and the Preceptor Table.

**Causal template.** Writing the full 11-row tibble with a `tribble()` is clearer than assembling it from `bind_rows()`, because the visual layout of the tibble matches the rendered table row for row. (Note the `"Data"` on the middle-blank row 4 and `"Preceptor"` on row 9.)

```r
population_tibble <- tibble::tribble(
  ~Source    , ~`Candidate`   , ~`Election Year`, ~`Lifespan if Win`, ~`Lifespan if Lose`, ~`Election Outcome`, ~`Election Age`, ~More ,
  "..."      , "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "...",
  "Data"     , "Frank Miller" , "1978"          , "73"              , "?"                , "Won"              , "55"           , "...",
  "Data"     , "Susan Davis"  , "1984"          , "?"               , "76"               , "Lost"             , "49"           , "...",
  "Data"     , "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "...",
  "Data"     , "David Brown"  , "1992"          , "68"              , "?"                , "Won"              , "58"           , "...",
  "..."      , "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "...",
  "Preceptor", "John Smith"   , "1975"          , "78"              , "75"               , "Won"              , "52"           , "...",
  "Preceptor", "Mary Johnson" , "1982"          , "82"              , "79"               , "Lost"             , "48"           , "...",
  "Preceptor", "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "...",
  "Preceptor", "Robert Wilson", "1990"          , "75"              , "81"               , "Won"              , "45"           , "...",
  "..."      , "..."          , "..."           , "..."             , "..."              , "..."              , "..."          , "..."
)

pop_title_footnote      <- "This table combines observed data (top block) with Preceptor Table expectations (bottom block)."
pop_units_footnote      <- "Data rows: [observed units]; Preceptor rows: [units we want to predict]. Missing rows represent the broader population (stability, representativeness)."
pop_outcome_footnote    <- "Observed lifespans for data rows. Question marks are unobserved counterfactuals (validity)."
pop_treatment_footnote  <- "Election outcomes from vote tallies. Close margins approximate random assignment (unconfoundedness)."
pop_covariates_footnote <- "Election age from campaign records for data rows; expected values for Preceptor rows."

gt::gt(population_tibble) |>
  gt::tab_header(title = "Population Table") |>
  gt::tab_spanner(label = "Unit/Time"         , id = "unit_span",
                  columns = c(`Candidate`, `Election Year`)) |>
  gt::tab_spanner(label = "Potential Outcomes", id = "outcome_span",
                  columns = c(`Lifespan if Win`, `Lifespan if Lose`)) |>
  gt::tab_spanner(label = "Treatment"         , id = "treatment_span",
                  columns = c(`Election Outcome`)) |>
  gt::tab_spanner(label = "Covariates"        , id = "covariates_span",
                  columns = c(`Election Age`, More)) |>
  gt::cols_align(align = "center", columns = gt::everything()) |>
  gt::cols_align(align = "left"  , columns = c(`Candidate`)) |>
  gt::cols_label(More = "...") |>
  gt::fmt_markdown(columns = gt::everything()) |>
  gt::tab_footnote(footnote = pop_title_footnote,
                   locations = gt::cells_title()) |>
  gt::tab_footnote(footnote = pop_units_footnote,
                   locations = gt::cells_column_spanners(spanners = "unit_span")) |>
  gt::tab_footnote(footnote = pop_outcome_footnote,
                   locations = gt::cells_column_spanners(spanners = "outcome_span")) |>
  gt::tab_footnote(footnote = pop_treatment_footnote,
                   locations = gt::cells_column_spanners(spanners = "treatment_span")) |>
  gt::tab_footnote(footnote = pop_covariates_footnote,
                   locations = gt::cells_column_spanners(spanners = "covariates_span"))
```

**Predictive variant.** Same three changes as for the Preceptor Table: rename the outcome spanner to `"Outcome"`, drop the `Treatment` spanner and its footnote, and reword the outcome footnote without the validity-via-counterfactuals framing.

---

## 11. Canonical definitions

These are the ground truth for the project's key definitions. Use the wording below verbatim as the `message` text in written-answer exercises that ask for a definition. Use the same wording (or a close paraphrase) in chapter prose.

### Four Cardinal Virtues

> *Wisdom, Justice, Courage, and Temperance.*

### Wisdom

> *Wisdom begins with a question and then moves on to the creation of a Preceptor Table and an examination of our data.*

### Justice

> *Justice concerns the Population Table and the four key assumptions which underlie it: validity, stability, representativeness, and unconfoundedness.*

### Courage

> *Courage creates the data generating mechanism.*

### Temperance

> *Temperance interprets the data generating mechanism and then uses it to answer, with the help of graphics, the question(s) with which we began. Humility reminds us that this answer is always false.*

### Rubin Causal Model

> *The [Rubin Causal Model](https://en.wikipedia.org/wiki/Rubin_causal_model) is an approach to the statistical analysis of cause and effect based on the framework of potential outcomes.*

### Potential outcome

> *A potential outcome is the outcome for an individual under a specified treatment. In a causal model there are at least two potential outcomes for each unit: the outcome under treatment and the outcome under control.*

### Causal effect

> *A causal effect is the difference between two potential outcomes.*

### Fundamental problem of causal inference

> *The fundamental problem of causal inference is that we can only observe one potential outcome.*

### Predictive versus causal models

> *Predictive models have only one outcome column. Causal models have more than one (potential) outcome column because we need more than one potential outcome in order to estimate a causal effect.*

### Units

> *Units are the rows, both in the Preceptor Table and in the data. They are determined by the original question, which also determines the quantity of interest.*

### Variables

> *Variables is the general term for the columns in both the Preceptor Table and the data. The term is more general still, since it may refer to data vectors we would like to have in order to answer the question but which are not available in the data.*

### Outcome

> *The outcome is the most important variable. It is determined by the question/QoI. By definition, it must be present in both the data and the Preceptor Table.*

### Covariates

> *Covariates is the general term for all the variables which are not the outcome. The term is used in three ways: all variables that might matter (whether in the data or not), all variables in the data other than the outcome, and the subset of those variables actually used in the model.*

### Treatment

> *A treatment is a covariate which we can, at least in theory, manipulate. Treatments appear in causal models, not predictive ones.*

### Quantity of Interest (QoI)

> *The Quantity of Interest is the number we want to estimate — the answer to a specific question. We almost always calculate a posterior probability distribution for the QoI, since in the real world we will never know it precisely.*

### Preceptor Table

> *A Preceptor Table is the smallest possible table of data with rows and columns such that, if there is no missing data, we can easily calculate the quantity of interest.*

### Preceptor Table, detailed

> *The rows of the Preceptor Table are the units. The outcome is at least one of the columns. If the problem is causal, there will be at least two (potential) outcome columns. The other columns are covariates. If the problem is causal, at least one of the covariates will be considered a treatment.*

### Population Table

> *The Population Table includes a row for each unit/time combination in the underlying population from which both the Preceptor Table and the data are drawn.*

### Validity

> *Validity is the consistency, or lack thereof, in the columns of the data set and the corresponding columns in the Preceptor Table.*

### Stability

> *Stability means that the relationship between the columns in the Population Table is the same for three categories of rows: the data, the Preceptor Table, and the larger population from which both are drawn.*

### Representativeness

> *Representativeness, or the lack thereof, concerns two relationships among the rows in the Population Table. The first is between the data and the other rows. The second is between the other rows and the Preceptor Table.*

### Unconfoundedness

> *Unconfoundedness means that the treatment assignment is independent of the potential outcomes, when we condition on pre-treatment covariates.*

### Data Generating Mechanism (DGM)

> *The Data Generating Mechanism is the final model, the one we use to answer the question. It is a model of the process by which the world generates the data we observe.*

### Preceptor's Posterior

> *Preceptor's Posterior is the posterior distribution we would calculate if every assumption we made in Wisdom and Justice were correct. It is the best posterior achievable with our data; it is not the truth.*

---

## 12. Knowledge drop library

Short prose fragments — typically one or two sentences — that go in the End of an exercise. Use verbatim where applicable; edit lightly where the exercise context calls for specifics. Organized by virtue. Cross-references to §11 indicate that the knowledge drop is a canonical definition already covered there.

Knowledge drops are deliberately short. Students won't read more than two sentences.

### 12.1 Introduction

**On spaced repetition.**
> *The best way to ensure that students remember these concepts more than a few months after the course ends is spaced repetition, although we focus more on the repetition than on the spacing.*

**On professional practice.**
> *Professionals keep their data science work in the cloud because laptops fail.*

**On the Rubin Causal Model.**
> *According to the Rubin Causal Model, there must be two (or more) potential outcomes for any discussion of causation to make sense. This is simplest to discuss when the treatment has only two different values, generating only two potential outcomes.*

**On continuous treatments.**
> *If the treatment variable is continuous (like a lottery payment), then there are lots and lots of potential outcomes, one for each possible value of the treatment variable.*

**On manipulability.**
> *Any data set can be used to construct a causal model as long as there is at least one covariate that we can, at least in theory, manipulate. It does not matter whether or not anyone did, in fact, manipulate it.*

**On models as a conceptual frame.**
> *The same data set can be used to create, separately, lots and lots of different models, both causal and predictive. We can just use different outcome variables and/or specify different treatment variables. This is a conceptual framework we apply to the data. It is never inherent in the data itself.*

**On difference ≠ subtraction.**
> *A causal effect is defined as the difference between two potential outcomes. "Difference" does not necessarily mean "subtraction" — many potential outcomes are not numbers.*

**On predictive models having no treatment.**
> *With a predictive model, each individual unit has only one observed outcome. There are not two potential outcomes because none of the covariates are treated as treatment variables. Instead, all covariates are assumed to be "fixed." Predictive models have no "treatments" — only covariates.*

**On predictive language.**
> *In predictive models, do not use words like "cause," "influence," "impact," or anything else which suggests causation. The best phrasing is in terms of "differences" between groups of units with different values for a covariate of interest.*

**On causal being within-row.**
> *Any causal connection means exploring the within-row difference between two potential outcomes. There's no need to consider other rows.*

**On the iterative question.**
> *This is the first version of the question. We will now create a Preceptor Table to answer the question. We may then revise the question given complexities discovered in the data. We then update the question and the Preceptor Table. And so on.*

### 12.2 Wisdom

Canonical definitions from §11 appropriate here: Wisdom, Preceptor Table, Preceptor Table (detailed), Units, Variables, Outcome, Covariates, Treatment, Quantity of Interest.

**The Tukey walk-away.**
> *The combination of some data and an aching desire for an answer does not ensure that a reasonable answer can be extracted from a given body of data.* — John W. Tukey

**On the Preceptor Table not being exhaustive.**
> *The Preceptor Table does not include all the covariates which you will eventually include in your model. It only includes, along with the outcome(s), covariates which are mentioned in your question.*

**On the Preceptor Table forcing clarity.**
> *Specifying the Preceptor Table forces us to think clearly about the units and outcomes implied by the question. The resulting discussion sometimes leads us to modify the question with which we started. No data science project follows a single direction. We always backtrack. There is always dialogue.*

**On modeling units but caring about aggregates.**
> *We model units, but we only really care about aggregates.*

**On the outcome compromise.**
> *The outcome variable that we really care about is often not the outcome variable which our data includes. This compromise — working with what we have rather than what we really want — is a part of most data science work in the real world.*

**On three usages of "covariates."**
> *The term "covariates" is used in at least three ways in data science. First, it is all the variables which might be useful, regardless of whether or not we have the data. Second, it is all the variables for which we have data. Third, it is the set of variables in the data which we end up using in the model.*

**On treatment as covariate.**
> *Remember that a treatment is just another covariate which, for the purposes of this specific problem, we are assuming can be manipulated, thereby creating two or more different potential outcomes for each unit.*

**On time not being instant.**
> *A Preceptor Table can never really refer to an exact instant in time since nothing is instantaneous in this fallen world.*

**On looking at data.**
> *You can never look at the data too much.* — Mark Engerman

### 12.3 Justice

Canonical definitions from §11 appropriate here: Justice, Population Table, Validity, Stability, Representativeness, Unconfoundedness.

**On Justice being about concerns.**
> *Justice is about concerns that you (or your critics) might have, reasons why the model you create might not work as well as you hope.*

**On validity as a columns-thing.**
> *Validity is always about the columns in the Preceptor Table and the data. Just because columns from these two different tables have the same name does not mean that they are the same thing.*

**On validity enabling the Population Table.**
> *In order to consider the Preceptor Table and the data to be drawn from the same population, the columns from one must have a valid correspondence with the columns in the other. Validity, if true (or at least reasonable), allows us to construct the Population Table, which is the first step in Justice.*

**On the Population Table being bigger.**
> *The Population Table is almost always much bigger than the combination of the Preceptor Table and the data, because if we can really assume that both are part of the same population, then that population must cover a broad universe of time and units.*

**On the arbitrary time unit.**
> *The exact time period used — whether hour, day, month, year, or whatever — is relatively arbitrary. The important thing to note is that the Population Table, unlike the Preceptor Table, covers a period of time over which things may change.*

**On stability being about parameters.**
> *A change in time or the distribution of the data does not, in and of itself, demonstrate a violation of stability. Stability is about the parameters: β₀, β₁, and so on. Stability means these parameters are the same in the data as they are in the population as they are in the Preceptor Table.*

**On stability as a time-thing.**
> *Stability is all about time. Is the relationship among the columns in the Population Table stable over time? The longer the time period between the data and the Preceptor Table, the more suspect stability becomes.*

**On representativeness ideal and reality.**
> *Ideally, we would like both the Preceptor Table and our data to be random samples from the population. Sadly, this is almost never the case.*

**On representativeness' cost.**
> *When representativeness is violated, the estimates for the model parameters will be biased.*

**On stability vs. representativeness.**
> *Stability looks across time periods. Representativeness looks within time periods.*

**The crispest summary.**
> *Validity is about the columns in our Population Table. Stability and representativeness are about the rows.*

**On unconfoundedness being causal-only.**
> *This assumption is only relevant for causal models. We describe a model as "confounded" if this is not true. The easiest way to ensure unconfoundedness is to assign treatment randomly.*

**On randomization failing.**
> *The great advantage of randomized assignment of treatment is that it guarantees unconfoundedness, if the randomization is done correctly. There is no way for treatment assignment to be correlated with anything, including potential outcomes, if treatment assignment is random, and if the experimental set up worked as designed. Sadly, in the real world, there are sometimes problems.*

### 12.4 Courage

Canonical definitions from §11 appropriate here: Courage, Data Generating Mechanism.

**On tidymodels.**
> *The [tidymodels](https://www.tidymodels.org/) framework is the most popular one in the R world for estimating models. [Tidy Modeling with R](https://www.tmwr.org/) by Max Kuhn and Julia Silge is a great introduction.*

**On dummy variables from a 2-level variable.**
> *A categorical variable (whether character or factor) like `sex` is turned into a 0/1 "dummy" variable which is then renamed something like `sexMale`. We can't have words in a mathematical formula, hence the need for dummy variables.*

**On dummy variables with N categories.**
> *The same dummy variable approach applies to a categorical covariate with N values. Such cases produce N−1 dummy 0/1 variables. The presence of an intercept in most models means that we can't have N categories. The "missing" category is incorporated into the intercept.*

**On more variables, less interpretability.**
> *The more variables we add, the more difficult it is to interpret the meaning of any particular coefficient. But interpretation also becomes less important. We don't really care about coefficients. We care about using our model to estimate quantities of interest.*

**On code being primary.**
> *In data science, we deal with words, math, and code, but the most important of these is code. We created the mathematical structure of the model and then wrote a model formula in order to estimate the unknown parameters.*

**On workspace awareness.**
> *Just because something exists in the tutorial (or in the QMD) does not mean that it is in the Console. You should be aware of what exists in Console World, which is generally called your "workspace."*

**On why easystats isn't in the QMD.**
> *We don't add easystats to the QMD because we are only using it for an interactive check of our fitted model. However, the [easystats ecosystem](https://easystats.github.io/easystats/) has a variety of interesting functions and packages which you might want to explore.*

**On `check_predictions()`.**
> *The purpose of `check_predictions()` is to compare your actual data (in green) with data that has been simulated from your fitted model — your data generating mechanism. If your DGM is reasonable, data simulated from it should not look too dissimilar from your actual data. Of course, it won't look exactly the same because of randomness. The actual data should be within the range of outcomes that your DGM simulates.*

**On the hat and the error term.**
> *First, we have replaced the parameters with our best estimates. Second, the left-hand side variable has a hat because this formula generates our estimated outcome. A hat indicates an estimated value.*

**On the DGM being a formula.**
> *A data generating mechanism is just a formula, something which we can write down and implement with computer code. Of course, there is randomness built into the DGM, but we won't worry about that detail for now.*

**On `broom`.**
> *`tidy()` is part of the [broom](https://broom.tidymodels.org/) package, used to summarize information from a wide variety of models.*

**On caching.**
> *Including `#| cache: true` causes Quarto to cache the results of the chunk. The next time you render your QMD, as long as you have not changed the code, Quarto will just load up the saved fitted object.*

**On no hypothesis tests.**
> *Null hypothesis testing is a mistake. There is only the data, the models, and the summaries therefrom.*

**On randomness.**
> *Randomness is intrinsic to this fallen world.*

### 12.5 Temperance

Canonical definitions from §11 appropriate here: Temperance, Preceptor's Posterior.

**On Courage handing off to Temperance.**
> *Courage gave us the data generating mechanism. Temperance guides us in the use of the DGM — or the "model" — we have created to answer the question(s) with which we began. We create posteriors for the quantities of interest.*

**On parameters being imaginary.**
> *In the end, we don't really care about parameters, much less how to interpret them. Parameters are imaginary, like unicorns. We care about answers to our questions. Parameters are tools for answering questions. In the modern world, all parameters are nuisance parameters.*

**On humility.**
> *We should be modest in the claims we make. The posteriors we create are never the "truth." The assumptions we made to create the model are never perfect. Yet decisions made with flawed posteriors are almost always better than decisions made without them.*

**On data science projects beginning with a decision.**
> *Data science projects begin with a decision which we face. To make that decision wisely, we would like to have good estimates of many unknown numbers. Yet, in order to make progress, we need to drill down to one specific question. This leads to the creation of a data generating mechanism, which can then be used to answer lots of questions.*

**On `predictions()`.**
> *`predictions()` returns a data frame with one row for each observation in the data set used to fit the model.*

**On `plot_predictions()` vs. `plot_comparisons()`.**
> *We are often just as interested in comparisons as in predictions. It is tempting to think we can deduce comparisons by subtracting one prediction from another. This mostly works for the center of the distribution but definitely not for the confidence interval. If you want the difference or ratio of more than one expected value, use `plot_comparisons()`.*

**On non-treatment variables in interpretation.**
> *Whenever we consider non-treatment variables, we must never use terms like "cause" or "impact." We can't make any statement which implies more than one potential outcome based on changes in non-treatment variables. We can only compare across rows. Use phrases like "when comparing X and Y."*

**On dummy variable base values.**
> *Dummy variables must always be interpreted in the context of the base value for that variable, which is generally included in the intercept. The base value for a character variable is the first alphabetically by default. For a factor, you can change this by setting the order of the levels by hand.*

**On same data, different assumptions.**
> *The interpretation of a treatment variable is very different from the interpretation of a standard covariate. There is no such thing as a causal data set (versus a predictive one), nor causal R code (versus predictive). You can use the same data set and the same R code for both. The difference lies in the assumptions you make.*

**On parameters not "meaning" anything.**
> *Most of the time parameters in a model have no direct relationship with any population value in which we might be interested. Especially in complex and non-linear models, a coefficient like β₀ does not "mean" anything. But in simple linear models, it sometimes corresponds to something real.*

**On confidence intervals excluding zero.**
> *We care if the confidence interval for a given variable excludes zero. If not, we can't be sure whether the relationship between the variable and the outcome is positive or negative. In that case, why would we include the variable in the model at all?*

**On "adjust" vs. "control."**
> *We recommend the verb "adjust" in place of "control" when discussing the effect of including other variables in the model. "The causal effect is 1.5, adjusting for age and party." "Adjusting" demonstrates humility; "controlling" does not.*

**On overlapping dummy intervals.**
> *If the variable is categorical, we care whether the confidence interval for one of the dummy columns overlaps with the confidence intervals for the other dummy columns derived from that categorical variable. If so, we can't be sure about the ordering of importance among the categories.*

**On comparisons with numeric variables.**
> *Numeric variables are harder to use in comparisons than binary variables because there are no longer two well-defined groups. We must create those two groups ourselves. As long as there are no interaction terms, we can pick two groups with any values. The most common two groups differ by one unit of the variable.*

**On back-and-forth in data science.**
> *Data science often involves back-and-forth work. First, make a single chunk of code — say, a new plot — work well. This requires interactive work between the QMD and the Console. Second, ensure that the entire QMD runs correctly on its own.*

**On the map and the territory.**
> *Always remember: the map is not the territory. A beautiful graphic tells a story, but that story is always an imperfect representation of reality. Our models depend on assumptions that are never completely true.*

**On going back to the Preceptor Table.**
> *Always go back to your Preceptor Table — the information which, if you had it, would make answering your question easy. In almost all real-world cases, the Preceptor Table and the data are fairly different. So, even a perfectly estimated statistical model is rarely as useful as we might like.*

**On the published version.**
> *This is the version of your QMD file at which your teacher is most likely to look closely.*

**On the Preceptor Table and God.**
> *We can never know all the entries in the Preceptor Table. That knowledge is reserved for God. If all our assumptions are correct, then our DGM is true — it accurately describes the way in which the world works. There is no better way to predict the future, or to model the past, than to use it. Sadly, this will only be the case with toy examples involving things like coins and dice.*

**On humility.**
> *We can never know the truth.*

**On the world's uncertainty.**
> *The world is always more uncertain than our models would have us believe.* (Last line of every chapter and the last line of every tutorial's Summary section.)

---

## 13. Master exercise list

This is the ordered list of exercises that make up an example tutorial. Each exercise is tagged:

- **[canonical]** — use the wording and `message` text below verbatim. These are the spaced-repetition backbone.
- **[per-tutorial]** — the question frame is fixed; the author writes a problem-specific prompt and/or `message`.
- **[operational]** — a workflow instruction (creating files, rendering, CP/CR). Always a written-without-answer exercise. These are migrated from the template as-is.

Within a section, keep exercises in the order given. Not every tutorial includes every exercise — the schedule depends on spaced repetition (§8) and on whether the problem is causal or predictive. Some exercises (e.g., unconfoundedness questions) are skipped entirely for predictive tutorials.

### 13.1 Introduction

Opens with a single "Imagine that you are …" paragraph that motivates the problem with a real person facing real decisions. Always starts with "Imagine that you are …" and always ends with "There are many decisions to make." The same paragraph is used in the matching chapter.

**Exercise 1.** [canonical] Four Cardinal Virtues.
- Prompt: *What are the four [Cardinal Virtues](https://en.wikipedia.org/wiki/Cardinal_virtues), in order, which we use to guide our data science work?*
- Message: `"Wisdom, Justice, Courage, and Temperance."`
- End: knowledge drop *On spaced repetition* (§12.1).

**Exercise 2.** [operational] Create a GitHub repo.
- Prompt: *Create a GitHub repo called `XX`. Make sure to click the "Add a README file" checkbox. Connect the repo to a project on your computer using `File -> New Folder from Git ...`. Select "Open in a new window." You need two editor windows: this one for the tutorial, and the one you just created for your code and Console. Select `File -> New File -> Quarto Document ...`. Provide a title — `"XX"` — and an author. Render the document and save it as `XX.qmd`. Create a `.gitignore` file with `XX_files` on the first line followed by a blank line. Save and push. In the Console, run `show_file(".gitignore")`. If that fails, it's probably because you haven't loaded `library(tutorial.helpers)` in the Console. CP/CR.*
- In the **first** tutorial, spell out CP/CR as "copy the Console output and paste it in the Response" on first use.
- End: *Professionals keep their data science work in the cloud because laptops fail.*

**Exercise 3.** [operational] Add libraries and echo settings to QMD.
- Prompt: *In your QMD, put `library(tidyverse)` and `library(<data package>)` in a new code chunk. Render the file. Notice that the file does not look good because the code is visible and there are messages. Add `#| message: false` to remove the messages in this setup chunk. Also add the following to the YAML header to remove all code echos from the HTML:*
  ```
  execute:
    echo: false
  ```
- *In the Console, run `show_file("XX.qmd", chunk = "Last")`. CP/CR.*
- End: *Render again. Everything looks nice, albeit empty, because we have added code to make the file look better and more professional.*

**Exercise 4.** [operational] `library(tidyverse)` via `Cmd/Ctrl + Enter`.
- Prompt: *Place your cursor in the QMD file on the `library(tidyverse)` line. Use `Cmd/Ctrl + Enter` to execute that line. Note that this copies the line to the Console and runs it. CP/CR.*
- (No End before next exercise; this runs into the next one naturally.)

**Exercise 5.** [operational] Next `library()` via `Cmd/Ctrl + Enter`.
- Prompt: *Place your cursor in the QMD file on the next `library()` line. Use `Cmd/Ctrl + Enter` to execute that line. This workflow — writing things in the QMD so you have a permanent copy, and then executing them in the Console with `Cmd/Ctrl + Enter` — is the most common approach to data science. There is QMD World and Console World. It is your responsibility to keep them in sync. CP/CR.*
- End: introduce the data: "A version of the data from XX is available in the `XX` tibble."

**Exercise 6.** [operational] Read the `?<tibble>` help.
- Prompt: *In the Console, type `?XX`, and paste the Description below.*
- Only include if the tibble has a help page. Delete if not.
- End: short paragraph of context about the data (paper abstract, source website quote).

**Exercise 7.** [canonical] Define a causal effect.
- Prompt: *Define a causal effect.*
- Message: `"A causal effect is the difference between two potential outcomes."`
- End: *According to the Rubin Causal Model, there must be two (or more) potential outcomes for any discussion of causation to make sense. This is simplest to discuss when the treatment has only two different values, generating only two potential outcomes.*

**Exercise 8.** [canonical] Fundamental problem of causal inference.
- Prompt: *What is the fundamental problem of causal inference?*
- Message: `"The fundamental problem of causal inference is that we can only observe one potential outcome."`
- End: *If the treatment variable is continuous (like a lottery payment), then there are lots and lots of potential outcomes, one for each possible value of the treatment variable.*

**Exercise 9.** [per-tutorial, written-with-answer] The outcome variable.
- Prompt: *XX is the broad topic of this tutorial. Given that topic, which variable in `<tibble>` should we use as our outcome variable?*
- Message: a sentence about the outcome variable used.
- End: *We will use `XX` as our outcome variable.* Follow with a simple AI-generated plot of the outcome (univariate or bivariate; bivariate should use a non-key covariate). Subtitle highlights an aspect of the data. No code chunk label.

**Exercise 10.** [per-tutorial, written-with-answer] An imaginary binary treatment.
- Prompt: *Let's imagine a brand new variable which does not exist in the data. This variable should be binary — it only takes on one of two values — and, at least in theory, manipulable. Describe this imaginary variable and how we might manipulate its value.*
- Message pattern: concrete example of such a variable with `` `backticks` `` around its name, e.g. *"Imagine a variable called `phone_call` which has value 1 if the person received a call urging them to vote and 0 otherwise. We, the organization making the calls, can manipulate this variable by deciding whether to call a specific individual."*
- (If this is a causal model, also include these two sentences in the Start: *For now, ignore the actual treatment variable `XX` which we will use later. The point of this exercise is to reinforce our understanding of the Rubin Causal Model.*)
- End: *Any data set can be used to construct a causal model as long as there is at least one covariate that we can, at least in theory, manipulate. It does not matter whether or not anyone did, in fact, manipulate it.*

**Exercise 11.** [per-tutorial, written-with-answer] Count potential outcomes for binary treatment.
- Prompt: *Given our (imaginary) treatment variable `XX`, how many potential outcomes are there for each unit? Explain why.*
- Message pattern: *"There are two potential outcomes because the treatment variable `XX` takes on two possible values: XX."*
- End: *The same data set can be used to create, separately, lots and lots of different models, both causal and predictive. This is a conceptual framework we apply to the data. It is never inherent in the data itself.*

**Exercise 12.** [per-tutorial, written-with-answer] Compute a unit-level causal effect.
- Prompt: *Specify two different values for the imaginary treatment variable `XX`, for a single unit; guess the potential outcomes; and calculate the causal effect for that unit given those guesses.*
- Message pattern: *"For a given [unit], assume the treatment variable could be [treatment] or [control]. If the unit gets [treatment], the outcome would be [value A]. If the unit gets [control], the outcome would be [value B]. The causal effect on the outcome of a treatment of [treatment] versus [control] is [value A] − [value B] = [difference], which is the causal effect for this unit."*
- End: *A causal effect is the difference between two potential outcomes. "Difference" does not necessarily mean "subtraction" — many potential outcomes are not numbers. Even for numeric outcomes, you can't simply say the effect is 10 without specifying the order of subtraction.*

**Exercise 13.** [per-tutorial, written-with-answer] Predictive covariate of interest.
- Prompt: *Let's consider a predictive model. Which variable in `<tibble>` do you think might have an important connection to `<outcome>`?*
- Message pattern: brief description of one key covariate whose connection we might want to explore.
- End: *With a predictive model, each individual unit has only one observed outcome. Predictive models have no "treatments" — only covariates.*

**Exercise 14.** [per-tutorial, written-with-answer] Two groups that might differ.
- Prompt: *Specify two different groups of [units] which have different values for [covariate] and which might have different average values for the [outcome].*
- Message pattern: *"Consider two groups: one with [covariate] = [value A], one with [covariate] = [value B]. These two groups might have different average values for the outcome."*
- End: *In predictive models, do not use "cause," "influence," "impact," or similar words. The best phrasing is in terms of "differences" between groups of units with different values for a covariate of interest.*

**Exercise 15.** [per-tutorial, written-with-answer] State the question.
- Prompt: *Write a [causal or predictive] question connecting the outcome `XX` to `XX`, the covariate of interest.*
- Message pattern: the specific question. Causal: "What is the average causal effect of [treatment] on [outcome]?" Predictive: "What is the difference in [outcome] between [group A] and [group B]?"
- End: *This is the first version of the question. We will now create a Preceptor Table to answer the question. We may then revise the question given complexities discovered in the data. And so on.*

### 13.2 Wisdom

Opens with one of four Wisdom quotes (pick one; see template for list) and a short paragraph narrowing from the broad "Imagine" topic to a plausibly answerable question. By the end of Wisdom, the student has a specific question.

**Exercise 1.** [canonical] Components of Wisdom.
- Prompt: *In your own words, describe the key components of Wisdom when working on a data science problem.*
- Message: `"Wisdom begins with a question and then moves on to the creation of a Preceptor Table and an examination of our data."`
- End: the Tukey walk-away quote (§12.2).

**Exercise 2.** [canonical] Define a Preceptor Table.
- Prompt: *Define a Preceptor Table.*
- Message: `"A Preceptor Table is the smallest possible table of data with rows and columns such that, if there is no missing data, we can easily calculate the quantity of interest."`
- End: *The Preceptor Table does not include all the covariates which you will eventually include in your model. It only includes, along with the outcome(s), covariates which are mentioned in your question.*

Between Exercises 2 and 3, insert at least two problem-specific EDA exercises (AI-prompted code, §9). One typically explores the outcome variable; the other explores the treatment (causal) or key covariate (predictive). Provide knowledge drops that highlight what the plot reveals.

**Exercise 3.** [canonical] Components of a Preceptor Table.
- Prompt: *Describe the key components of Preceptor Tables in general, without worrying about this specific problem. Use words like "units," "outcomes," and "covariates."*
- Message: `"The rows of the Preceptor Table are the units. The outcome is at least one of the columns. If the problem is causal, there will be at least two (potential) outcome columns. The other columns are covariates. If the problem is causal, at least one of the covariates will be considered a treatment."`
- End (causal): *This problem is causal, so one of the covariates is a treatment. In our problem, the treatment is XX. There is a potential outcome for each of the XX possible values of the treatment.*
- End (predictive): a sentence comparing outcomes for two different groups.

**Exercise 4.** [per-tutorial, written-with-answer] The units.
- Prompt: *What are the units for this problem?*
- Message: per-tutorial specification of the unit (and implicit time period).
- End: *Specifying the Preceptor Table forces us to think clearly about the units and outcomes implied by the question. No data science project follows a single direction. We always backtrack. There is always dialogue. We model units, but we only really care about aggregates.*

**Exercise 5.** [per-tutorial, written-with-answer] The outcome variable.
- Prompt: *What is the outcome variable for this problem?*
- Message pattern: *"Keep track of two 'outcome' variables: the one in our Preceptor Table and the one in our data. In this case, XX."*
- End: the outcome-compromise knowledge drop (§12.2).

**Exercise 6.** [per-tutorial, written-with-answer] A plausible covariate.
- Prompt: *What is a covariate which you think might be useful for this problem, regardless of whether or not it might be included in the data?*
- Message pattern: a sensible variable plausibly connected to the outcome.
- End: the three-usages-of-covariates knowledge drop (§12.2).

**Exercise 7.** [per-tutorial, written-with-answer] The treatment(s).
- Prompt: *What are the treatments, if any, for this problem?*
- Message: per-tutorial specification of the treatment variable, or a note that this is a predictive model with no treatment.
- End: the treatment-as-covariate knowledge drop (§12.2).

**Exercise 8.** [per-tutorial, written-with-answer] Moment in time.
- Prompt: *What moment in time does the Preceptor Table refer to?*
- Message: one brief phrase or sentence describing the moment.
- End: *A Preceptor Table can never really refer to an exact instant in time since nothing is instantaneous in this fallen world.* Optionally followed by a more sophisticated sentence tying to the specific problem.

Between Exercises 8 and 9, consider inserting an AI-prompted plot with outcome on the y-axis and the most important covariate/treatment on the x-axis. Knowledge drop after: the Engerman "you can never look at the data too much" quote (§12.2).

**Exercise 9.** [per-tutorial, written-with-answer] Describe the Preceptor Table in words.
- Prompt: *Describe in words the Preceptor Table for this problem.*
- Message: an excellent description mentioning *rows*, *units*, *outcome*, *covariates*, and (if causal) *treatment*.
- End: *The Preceptor Table for this problem looks something like this:* — then insert a rendered `gt` Preceptor Table (§10). Followed by: *Like all aspects of a data science problem, the Preceptor Table evolves as we continue our work.*

If the modeling requires a cleaned tibble `x` (e.g., filtering to one year, dropping NAs), insert 1–3 AI-prompted code exercises that build the cleaned `x`. Follow with a QMD-world exercise that asks the student to copy/paste the pipeline into a new code chunk, render, and `show_file("XX.qmd", chunk = "Last")`.

**Exercise 10.** [per-tutorial, written-with-answer] The narrow specific question.
- Prompt: *What is the narrow, specific question we will try to answer?*
- Message: per-tutorial.
- End: *The answer to this question is your Quantity of Interest. It is OK if your question differs from ours. Many similar questions lead to the creation of the same model.*

**Exercise 11.** [per-tutorial, written-with-answer] First two sentences of the summary paragraph.
- Prompt: *We will be creating a summary paragraph over the course of this tutorial. Write the first two sentences. The first sentence is a general statement about the overall topic, mentioning the general class of outcome and at least one covariate. The second introduces the data source and the specific question — when/where gathered, how many observations, who collected it.*
- Message: an excellent two-sentence opener.
- End: *Read our answer. It will not be the same as yours. You can change your answer to incorporate some of our ideas, but do not copy/paste our answer exactly. Add your two sentences to your QMD, `Cmd/Ctrl + Shift + K`, and commit/push.*

### 13.3 Justice

Opens with one of five Justice quotes (pick one; see template).

**Exercise 1.** [canonical] Components of Justice.
- Prompt: *In your own words, name the five key components of Justice when working on a data science problem.*
- Message: `"Justice concerns the Population Table and the four key assumptions which underlie it: validity, stability, representativeness, and unconfoundedness."`
- End: *Justice is about concerns that you (or your critics) might have, reasons why the model you create might not work as well as you hope.*

**Exercise 2.** [canonical] Define validity.
- Prompt: *In your own words, define "validity" as we use the term.*
- Message: `"Validity is the consistency, or lack thereof, in the columns of the data set and the corresponding columns in the Preceptor Table."`
- End: the validity-as-columns knowledge drop (§12.3).

**Exercise 3.** [per-tutorial, written-with-answer] A validity concern for this problem.
- Prompt: *Provide one reason why the assumption of validity might not hold for the outcome variable `XX` or for one of the covariates. Use the words "column" or "columns" in your answer.*
- Message pattern: specific concern about outcome and/or covariate columns. Our answer should be longer than the one we expect from students, ideally mentioning both.
- End: *In order to consider the Preceptor Table and the data to be drawn from the same population, the columns from one must have a valid correspondence with the columns in the other. Validity, if true (or at least reasonable), allows us to construct the Population Table, which is the first step in Justice.* Optionally also: *Because we control the Preceptor Table and, to a lesser extent, the original question, we can adjust those variables to be closer to the data we actually have.*

**Exercise 4.** [canonical] Define a Population Table.
- Prompt: *In your own words, define a Population Table.*
- Message: `"The Population Table includes a row for each unit/time combination in the underlying population from which both the Preceptor Table and the data are drawn."`
- End: the Population Table is bigger knowledge drop (§12.3).

**Exercise 5.** [per-tutorial, written-with-answer] The unit/time combination.
- Prompt: *Specify the unit/time combinations which define each row in this Population Table.*
- Message: per-tutorial.
- End: the arbitrary-time-unit knowledge drop (§12.3). Follow with a rendered `gt` Population Table (§10).

**Exercise 6.** [canonical] Define stability.
- Prompt: *In your own words, define the assumption of "stability" when employed in the context of data science.*
- Message: `"Stability means that the relationship between the columns in the Population Table is the same for three categories of rows: the data, the Preceptor Table, and the larger population from which both are drawn."`
- End: the stability-as-time knowledge drop (§12.3).

**Exercise 7.** [per-tutorial, written-with-answer] A stability concern for this problem.
- Prompt: *Provide one reason why the assumption of stability might not be true in this case.*
- Message: per-tutorial. Students read these "official" answers carefully — make the example precise.
- End: the stability-about-parameters knowledge drop (§12.3).

**Exercise 8.** [canonical] Define representativeness.
- Prompt: *We use our data to make inferences about the overall population. We use information about the population to make inferences about the Preceptor Table: Data → Population → Preceptor Table. In your own words, define the assumption of "representativeness."*
- Message: `"Representativeness, or the lack thereof, concerns two relationships among the rows in the Population Table. The first is between the data and the other rows. The second is between the other rows and the Preceptor Table."`
- End: the representativeness-ideal-and-reality knowledge drop (§12.3).

**Exercise 9.** [per-tutorial, written-with-answer] Representativeness: data vs. population.
- Prompt: *We do not use the data directly to estimate missing values in the Preceptor Table. Instead, we use the data to learn about the overall population. Provide one reason, involving the relationship between the data and the population, why the assumption of representativeness might not be true in this case.*
- Message: per-tutorial. Do not invoke time — save time-based examples for stability.
- End: the representativeness-cost knowledge drop (§12.3).

**Exercise 10.** [per-tutorial, written-with-answer] Representativeness: population vs. Preceptor Table.
- Prompt: *We use information about the population to make inferences about the Preceptor Table. Provide one reason, involving the relationship between the Population and the Preceptor Table, why the assumption of representativeness might not be true in this case.*
- Message: per-tutorial. Again, avoid time.
- End: the stability-vs-representativeness knowledge drop (§12.3).

**Exercise 11.** [canonical, causal only] Define unconfoundedness.
- Prompt: *In your own words, define the assumption of "unconfoundedness" when employed in the context of data science.*
- Message: `"Unconfoundedness means that the treatment assignment is independent of the potential outcomes, when we condition on pre-treatment covariates."`
- End: the unconfoundedness-being-causal-only knowledge drop (§12.3).

**Exercise 12.** [per-tutorial, written-with-answer, causal only] An unconfoundedness concern.
- Prompt: *Provide one reason why the assumption of unconfoundedness might not be true (or relevant) in this case.*
- Message: per-tutorial. Confounds are the hardest questions for students — your example must be good, specifying precisely how treatment assignment is correlated with potential outcomes.
- End: the randomization-failing knowledge drop (§12.3).

**Exercise 13.** [operational] Load the modeling package.
- Prompt: *A statistical model consists of two parts: the probability family and the link function. The probability family is the probability distribution which generates the randomness in our data. The link function is the mathematical formula which links our data to the unknown parameters. Add `library(tidymodels)` to the QMD file. Place your cursor on that line. Use `Cmd/Ctrl + Enter` to execute. Note that this copies the line to the Console. CP/CR.*
- End: the probability family is determined by the outcome variable $Y$. Pick the relevant one:
  - Continuous → Normal: $Y \sim N(\mu, \sigma^2)$
  - Binary → Bernoulli: $Y \sim \text{Bernoulli}(\rho)$
  - Multi-category unordered → Multinomial
  - Multi-category ordered → Cumulative Logistic
  Full LaTeX for each lives in the template; migrate as needed.

**Exercise 14.** [operational] Add `library(broom)`.
- Prompt: *Add `library(broom)` to the QMD file. Place your cursor on that line. Use `Cmd/Ctrl + Enter`. CP/CR.*
- In later tutorials, shrink the prompt verbiage.
- End: the link function depends on the outcome variable type. Pick the relevant functional form:
  - Continuous → linear: $\mu = \beta_0 + \beta_1 X_1 + \ldots$
  - Binary → log-odds: $\log[\rho / (1 - \rho)] = \beta_0 + \beta_1 X_1 + \ldots$
  - Multinomial → multinomial logistic (three-outcome form in template)
  - Ordinal → cumulative logistic (three-outcome form in template)

**Exercise 15.** [per-tutorial, written-without-answer] LaTeX for the model.
- Prompt: *Use AI to come up with a LaTeX representation of the mathematical structure of the model, with $Y$ as the dependent variable and $X_1, X_2, \ldots$ as independent variables. This version has no parameter values since we haven't estimated them. Confirm the LaTeX works by placing it in your QMD and rendering. Paste that LaTeX below.*
- End: *Our answer:* followed by a nice LaTeX display (normal/Bernoulli/multinomial/cumulative as appropriate), then the source LaTeX inside a 4-backtick block. Closing knowledge drop: *We use generic variables — $Y$, $X_1$, and so on — because our purpose is to describe the general mathematical structure of the model, independent of the specific variables we will eventually use. Having decided on the basic mathematical structure, we now turn toward estimating the model.*

**Exercise 16.** [per-tutorial, written-with-answer] Add a weakness sentence to the summary.
- Prompt: *Write one sentence highlighting a potential weakness in your model. Derive it from possible problems with the assumptions above. We will add this to our summary paragraph. So far our version of the summary paragraph looks like this:* (paste our first two sentences). *Your version will be somewhat different.*
- Message: per-tutorial.
- End: *Add a weakness sentence to the summary paragraph in your QMD. You can modify your paragraph, but don't copy/paste our answer exactly. `Cmd/Ctrl + Shift + K`, then commit/push.*

### 13.4 Courage

Opens with one of four Courage quotes.

**Exercise 1.** [canonical] Components of Courage.
- Prompt: *In your own words, describe the components of the virtue of Courage for analyzing data.*
- Message: `"Courage creates the data generating mechanism."`
- End: *Having decided on the basic mathematical structure of the model at the end of Justice — a choice mostly driven by the distribution of our outcome variable — we now turn toward estimating the model.*

**Exercise 2.** [per-tutorial, code] Start the model.
- Prompt: *Because our outcome variable is [binary/continuous/multinomial/ordinal], start to create the model by entering `<appropriate model function>`* (e.g., `linear_reg(engine = "lm")`, `logistic_reg(engine = "glm")`, `multinom_reg(engine = "glmnet")`).
- End: the tidymodels knowledge drop (§12.4).

**Exercise 3.** [per-tutorial, code] Fit a single-variable model.
- Prompt: *Continue the pipe to `fit(<outcome> ~ <single categorical covariate>, data = <tibble>)`.* Use a two-level categorical if possible.
- Copy-previous-code button is included.
- End: the dummy-variables-from-2-level knowledge drop (§12.4), customized with the actual variable name.

**Exercise 4.** [per-tutorial, code] Add `tidy(conf.int = TRUE)`.
- Prompt: *Continue the pipe with `tidy(conf.int = TRUE)`.*
- End: discuss the meaning of the intercept and β₁ using the actual numeric results.

**Exercise 5.** [per-tutorial, code] Fit with a 3+ level categorical variable.
- Prompt: *Change the call to `fit(<outcome> ~ <3+ level categorical>, data = <tibble>)`.*
- End: the dummy-variables-with-N-categories knowledge drop (§12.4), customized.

**Exercise 6.** [per-tutorial, code] Fit the final model.
- Prompt: *Change the call to `fit(<final formula>, data = <tibble>)`.*
- End: the more-variables-less-interpretability knowledge drop (§12.4). Add commentary about why this model was chosen.

**Exercise 7.** [per-tutorial, code] Display the fit object.
- Prompt: *Behind the scenes, an object called `fit_<n>` has been created. Type `fit_<n>` and hit "Run Code."*
- End: the code-being-primary knowledge drop (§12.4).

**Exercise 8.** [operational] Bring `fit_<n>` into Console World.
- Prompt: *We need `fit_<n>` to exist in Console World. Copy/paste this code into the Console and execute it:*
  ```
  fit_<n> <- <model spec> |>
    fit(<formula>, data = <tibble>)
  ```
- *CP/CR.*
- End: the workspace-awareness knowledge drop (§12.4).

**Exercise 9.** [operational] Load easystats in the Console.
- Prompt: *In the Console, load the [easystats](https://easystats.github.io/easystats/) package. CP/CR.*
- End: the why-easystats-isn't-in-the-QMD knowledge drop (§12.4).

**Exercise 10.** [operational] Run `check_predictions()`.
- Prompt: *In the Console, run `check_predictions(extract_fit_engine(fit_<n>))`. CP/CR.*
- End: the `check_predictions()` knowledge drop (§12.4). Add a sentence noting whether the simulated data looks like the actual data for this problem.

**Exercise 11.** [per-tutorial, written-without-answer] LaTeX for the fitted model.
- Prompt: *Ask AI to create LaTeX for this model, including variable names and estimates for all coefficients. Since this is a fitted model, the dependent variable has a hat and no error term. Add the code to your QMD, `Cmd/Ctrl + Shift + K`, check the formatting (no absurd decimals), and paste the LaTeX below.*
- End: *Our formula looks like:* <LaTeX>. *It was created with:* <backticked source>. Then the hat-and-error-term knowledge drop (§12.4), followed by: *This is our data generating mechanism.* Then the DGM-being-a-formula knowledge drop (§12.4).

**Exercise 12.** [operational] Cache the fit in the QMD.
- Prompt: *Create a new code chunk in your QMD. Add the chunk option `#| cache: true`. Copy/paste the R code for the final model into the chunk, assigning the result to `fit_<n>`. (This includes `fit()` but not `tidy()`.) Place your cursor on the `fit_<n>` line and use `Cmd/Ctrl + Enter`. (This is technically unnecessary since we already have `fit_<n>` in the workspace, but ensuring everything in the QMD is also in the Console is good habit.) `Cmd/Ctrl + Shift + K`. Rendering may be slow the first time but cached thereafter. At the Console, run `tutorial.helpers::show_file("XX.qmd", chunk = "Last")`. CP/CR.*
- End: the caching knowledge drop (§12.4). *To confirm, `Cmd/Ctrl + Shift + K` again. It should be quick.*

**Exercise 13.** [operational] Add `*_cache` to `.gitignore`.
- Prompt: *Add `*_cache` to `.gitignore`. Cached objects are often large and don't belong on GitHub. At the Console, run `tutorial.helpers::show_file(".gitignore")`. CP/CR.*
- End: *Because of the change in your `.gitignore` (assuming you saved it), the cache directory should not appear in the Source Control panel because Git is ignoring it. Commit and push.*

**Exercise 14.** [per-tutorial, code] Run `tidy(fit_<n>, conf.int = TRUE)`.
- Prompt: *In the Console, run `tidy()` on `fit_<n>` with `conf.int = TRUE`. This returns 95% intervals for all the parameters.*
- End: the `broom` knowledge drop (§12.4).

**Exercise 15.** [per-tutorial, written-without-answer] Make a nice table from `tidy()`.
- Prompt: *Create a new code chunk in your QMD. Ask AI to make a nice-looking table from the tibble returned by `tidy()`. You don't need all the columns — estimate and confidence intervals is typical. You may need to load [tinytable](https://vincentarelbundock.github.io/tinytable/), [knitr](https://yihui.org/knitr/), [gt](https://gt.rstudio.com/), [kableExtra](https://haozhu233.github.io/kableExtra/), [flextable](https://davidgohel.github.io/flextable/), or [modelsummary](https://modelsummary.com/) in the setup chunk. Insert your table code. `Cmd/Ctrl + Shift + K`. At the Console, `tutorial.helpers::show_file("XX.qmd", chunk = "Last")`. CP/CR.*
- End: show our table and our code. Closing knowledge drop: *At the very least, your table should include a title and a caption with the data source. The more you use AI, the better you will get at doing so.*

**Exercise 16.** [per-tutorial, written-with-answer] Model-structure sentence.
- Prompt: *Add a sentence to your project summary explaining the structure of the model. Something like: "I/we model XX [concept of outcome, not variable name], [values of XX], as a [linear/logistic/multinomial/ordinal] function of XX [and maybe other covariates]." Recall the beginning of our summary: [paste what we suggested at the end of Justice].*
- Message: per-tutorial.
- End: *Read our answer. Do not copy/paste exactly. Add your two sentences to the summary paragraph. `Cmd/Ctrl + Shift + K`, then commit/push.*

### 13.5 Temperance

Opens with one of six Temperance quotes.

**Exercise 1.** [canonical] Components of Temperance.
- Prompt: *In your own words, describe the use of Temperance in data science.*
- Message: `"Temperance interprets the data generating mechanism and then uses it to answer, with the help of graphics, the question(s) with which we began. Humility reminds us that this answer is always false."`
- End: the Courage-handoff knowledge drop (§12.5).

**Exercises 2–4.** [per-tutorial, written-with-answer] Interpret the parameters.
- Prompt pattern: show the `tidy(conf.int = TRUE)` table, ask an interpretation question about one parameter or one comparison. At least three such questions. See the *Temperance example knowledge drops* in §12.5 for options to use as the End of each. In tutorials using models with no interpretable parameters (random forests), cut these.
- Message: per-tutorial; must be excellent.
- End: pick from the Temperance knowledge drops (non-treatment variables, dummy base values, confidence intervals excluding zero, same-data-different-assumptions, parameters-don't-mean-anything, "adjust" vs. "control," overlapping dummy intervals, numeric comparisons).

**Exercise 5.** [operational] Load `marginaleffects`.
- Prompt: *In the end, we don't really care about parameters. Parameters are imaginary, like unicorns. We care about answers to our questions. In the modern world, all parameters are nuisance parameters. Add `library(marginaleffects)` to the QMD. Place your cursor on that line. Use `Cmd/Ctrl + Enter`. CP/CR.*
- End: the humility knowledge drop (§12.5).

**Exercise 6.** [per-tutorial, written-with-answer] The specific question.
- Prompt: *What is the specific question we are trying to answer?*
- Message: per-tutorial. May match the Wisdom Exercise 10 question or differ.
- End: the data-science-projects-begin-with-decisions knowledge drop (§12.5).

**Exercise 7.** [per-tutorial, written-without-answer] Run `predictions()`.
- Prompt: *In the Console, run `predictions()` on `fit_<n>`. CP/CR.*
- End: the `predictions()` knowledge drop (§12.5), noting actual row count. Add a second sentence specific to what's interesting about this output.

**Exercise 8.** [per-tutorial, written-without-answer] Run `plot_predictions()` (first version).
- Prompt: *In the Console, run `plot_predictions()` on `fit_<n>` with [specific arguments]. CP/CR.*
- End: discuss the estimate and uncertainty the plot shows. Explain how to read the estimate and confidence interval from the plot.

Insert additional `plot_predictions()` exercises as needed — different arguments, options like `points`, or `draw = FALSE` to return a tibble. `plot_comparisons()` belongs here when the question calls for differences rather than level estimates (see §12.5).

**Exercise 9.** [per-tutorial, written-without-answer] Final `plot_predictions()` call.
- Prompt: the version whose output will be the basis for the final plot. CP/CR.
- End: comments on the key takeaways relative to the question.

**Exercise 10.** [per-tutorial, written-without-answer] `plot_predictions(..., draw = FALSE)`.
- Prompt: run the same call as above with `draw = FALSE`. CP/CR.
- End: *Because `plot_predictions()` returns a ggplot object, you can continue with ggplot commands like `labs()`. But it can be useful to see the underlying values in the tibble and build your own plot directly.*

**Exercise 11.** [per-tutorial, written-without-answer] Build a beautiful plot.
- Prompt: *Work with AI to create a beautiful plot starting from the output of `plot_predictions(..., draw = FALSE)`. Do this in your QMD (much easier than the Console). Title: key variables. Subtitle: important takeaway. Caption: data source. Axis labels: nice. This plot is not directly connected to your question — it answers lots of questions. Paste the plot code below.*
- End: show our plot and our code. Closing knowledge drop: the back-and-forth knowledge drop (§12.5).

**Exercise 12.** [operational] Finalize the plot chunk.
- Prompt: *Finalize the new graphics chunk in your QMD. `Cmd/Ctrl + Shift + K` to ensure it all works. At the Console, `tutorial.helpers::show_file("XX.qmd", chunk = "Last")`. CP/CR.*
- End: the map-is-not-the-territory knowledge drop (§12.5).

**Exercise 13.** [per-tutorial, written-with-answer] Last sentence of the summary paragraph.
- Prompt: *Write the last sentence of your summary paragraph. It describes at least one Quantity of Interest and a measure of uncertainty. It is OK if this QoI differs from the one you began with. It is OK to discuss more than one QoI.*
- Message: per-tutorial.
- End: *Add a final sentence to your summary paragraph, but don't copy/paste our answer exactly. `Cmd/Ctrl + Shift + K`.*

**Exercise 14.** [per-tutorial, written-with-answer] Why the estimate might be wrong.
- Prompt: *Write a few sentences explaining why the estimates for the quantities of interest, and the uncertainty, might be wrong. Suggest alternative estimates and confidence interval if warranted.*
- Message: per-tutorial. *You might or might not suggest an alternate point estimate; I always adjust toward my subjective sense of a long-run average or zero. But you should always widen the confidence interval, since the assumptions of your model are always false.*
- End: the go-back-to-the-Preceptor-Table knowledge drop (§12.5).

**Exercise 15.** [operational] Reorder and render final QMD.
- Prompt: *Rearrange the material in your QMD so the order is graphic, then paragraph. The chunk that creates the fitted model must occur before the chunk that creates the graphic. You can keep or discard the math at your discretion. `Cmd/Ctrl + Shift + K`. At the Console, `tutorial.helpers::show_file("XX.qmd")`. CP/CR.*
- End: the published-version knowledge drop (§12.5).

**Exercise 16.** [operational] Publish to GitHub Pages.
- Prompt: *Publish your rendered QMD to GitHub Pages. In the Terminal (not the Console!), run `quarto publish gh-pages XX.qmd`. Copy/paste the resulting URL below.*
- End: *Commit/push everything.*

**Exercise 17.** [operational] Paste the repo URL.
- Prompt: *Copy/paste the URL to your GitHub repo.*
- End: the Preceptor-Table-and-God knowledge drop (§12.5), followed by *The world confronts us. Make decisions we must.*

### 13.6 Summary

Not an exercise sequence — a brief closing prose section. Give the final plot and the summary paragraph. Add concerns separately. Note how the findings might help the "Imagine" person from the Introduction, and mention other numbers that would be useful. End with: *The world is always more uncertain than our models would have us believe.*

Then the `download_answers.Rmd` child document:

```r
```{r download-answers, child = system.file("child_documents/download_answers.Rmd", package = "tutorial.helpers")}
```
```

---

## 14. Guidance for tutorial authors

General rules and patterns pulled from the template that apply across exercises, rather than to any one exercise.

### 14.1 Answers that students read closely

Students read our `message` text as closely as they read anything in the tutorial. They compare their own answer to ours. Make our answer excellent. This is especially true for Justice answers about validity, stability, representativeness, and unconfoundedness — students look here for examples of how to reason about assumptions.

Where a knowledge drop has two components — a universal truth and a problem-specific example — separate them when practical. The universal truth belongs in §12. The problem-specific piece is written per-tutorial.

### 14.2 Build objects through questions, not by assignment

Never have students do the final assignment themselves. Build objects through a series of exercises — often a pipe built line by line — each producing output the student can examine. When the creation is complete, have a final exercise that says, in effect: "Behind the scenes we have assigned the result to the object `fit_<n>`. To confirm, type `fit_<n>` and hit Run Code."

With AI-mediated authoring (§9), the line-by-line pipe pattern is less necessary than it was. But for key objects you want the student to understand in parts, still prefer build-up over final-assignment.

### 14.3 Show the plot, don't show the code

For any tutorial-owned plot (EDA plots, a `gt` Preceptor Table, a quote-framed image), render the plot in a code chunk with no label, so the student sees the output but not the code. Plots should be competent — axis labels, a title, a subtitle that highlights the key takeaway, a caption naming the data source.

### 14.4 Iterative summary paragraph

The summary paragraph is built up across the tutorial:

- Wisdom Exercise 11: first two sentences (topic; data + question).
- Justice Exercise 16: add a weakness sentence.
- Courage Exercise 16: add a model-structure sentence.
- Temperance Exercise 13: add the final sentence with a QoI and uncertainty.

After each addition, the student updates their QMD, renders with `Cmd/Ctrl + Shift + K`, and commits/pushes.

### 14.5 Test chunks for marginaleffects calls

For `predictions()` and `plot_predictions()` exercises, include a `-test` chunk. These calls sometimes break with package updates; the test chunk is how we catch that.

### 14.6 Quotes at the top of each virtue section

Each virtue section opens with a thematic quote. The template lists options for each (four for Wisdom, five for Justice, four for Courage, six for Temperance). Pick one per tutorial; variety across the curriculum is good.

### 14.7 When to skip exercises

Predictive tutorials skip the unconfoundedness exercises (Justice 11 and 12). Models with no interpretable parameters (random forests, neural nets) skip the parameter-interpretation exercises (Temperance 2–4) — replace with a single exercise that ensures the student understands the parameters aren't directly interpretable.

Operational exercises can be abbreviated in later tutorials once students have done them a few times. The first time through, migrate as-is. By tutorial 10+, the `library(broom)` exercise (Justice 14) can be much shorter.

---

## 15. Curriculum and tutorial index

This section is a registry. It tracks what chapters exist, what datasets they use, and which recurring questions each tutorial has asked. It is how we execute spaced repetition in §8.

**Status: not yet populated.** Before writing a new tutorial, David and Claude should fill in the rows for existing tutorials so that Claude knows what has and has not been asked recently.

| Chapter | Type | Topic | Dataset | Model (tutorial) | Canonical exercises used |
|--------:|------|-------|---------|------------------|--------------------------|
| 01 | misc | Mechanics | — | — | — |
| 02 | misc | Probability | — | — | — |
| 03 | misc | Sampling | — | — | — |
| 04 | misc | Cardinal Virtues | — | — | — |
| 05 | misc | Rubin Causal Model | — | — | — |
| 06+ | example | TBD | TBD | TBD | TBD |

(David: please fill this in, or tell Claude which existing file to read it off of.)

---

## 16. R tooling

The tutorial setup chunk (§5.2) loads the full package stack. For chapters, setup is simpler: load the packages, fit the model, move on.

**Packages loaded for rendering only** (not expected in student's Console): `learnr`, `tutorial.helpers`, `gt`.

**Packages students are expected to load themselves** (and appear in the setup chunk for the tutorial to work):
- `tidyverse` — always.
- `tidymodels` — for most models. Replace with `ordinal` or another package if that model framework doesn't fit.
- `broom` — for tidying model output. `broom.mixed` for mixed models.
- `marginaleffects` — for `predictions()`, `plot_predictions()`, `plot_comparisons()`.
- `easystats` — for `check_predictions()`. Not added to student QMDs; used interactively in the Console.

**Data package**: whichever one holds the tutorial's dataset (`primer.data` most of the time).

**Setup chunk must be cheap**. A model fit that takes more than a few seconds breaks the student's flow. Use a subset of the data if needed; fit the full model with `#| cache: true` in the tutorial body rather than in setup.

---

## 17. Open items

Things flagged but not yet resolved. Revisit when relevant.

- **Assignment mechanism and selection mechanism** are not yet defined here as first-class concepts distinct from unconfoundedness. The template's bottom TODO list asks for Justice exercises on them; there are two selection mechanisms worth distinguishing (how a unit got into the data vs. how a unit got into the Preceptor Table). When ready, add definitions to §11 and decide where they fit in the Justice flow.
- **Mathematics subsection** appears in the `cardinal_virtues.qmd` Justice skeleton but is not written anywhere. Decide whether it is its own Justice subsection or folded into Courage.
- **Functional form guidance** for Courage — the rule of thumb is continuous outcome → linear, binary → logistic, multi-category unordered → multinomial logistic, multi-category ordered → cumulative logistic. Full LaTeX forms are in the template; migrate into §13.3 Exercise 13–15 when writing the first chapter that uses each.
- **Posterior predictive checks** — the `check_predictions()` flow in §13.4 Exercise 10 currently just checks and continues. Codify when to accept the DGM vs. when to go back and fit a new model, once a chapter actually does the latter.
- **Knowledge-drop splitting** — many template knowledge drops were one paragraph combining a universal truth with a problem-specific example (see §14.1). As we write new tutorials we should split them: universal truth to §12, problem-specific to the tutorial. A few of these splits are still pending.
- **Non-hypothesis-testing language** — the Courage section says no hypothesis tests, but we haven't codified what replaces a "p < 0.05" phrasing when reporting a coefficient's sign. Likely: "the confidence interval excludes zero" — but there should be a knowledge drop that handles this recurring moment.
- **The DGM randomness detail** — we defer discussion of the randomness in the DGM in Courage Exercise 11's knowledge drop. Decide in which tutorial this gets unwrapped.
- **"Console World" terminology** — the template uses "Console World" and "QMD World" heavily. These read fine in RStudio/Positron but awkward in VS Code on Codespaces. Whether to rename ("R World"?) is an open call.
- **AI tool article absorption** — `https://ppbds.github.io/tutorial.helpers/articles/ai.html` may have more AI-workflow specifics than §9 currently captures. Worth re-reading and absorbing the next time we touch §9.
- **Tutorial directory layout** — §3 refers to `inst/tutorials/NN-topic-name/tutorial.Rmd` but this should be confirmed against the current state of the `primer.tutorials` package.
- **Curriculum registry (§15)** — populate with actual chapter/tutorial state so spaced repetition can work.