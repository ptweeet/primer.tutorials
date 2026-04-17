# CLAUDE.md

This file is the working reference for writing chapters of the textbook *Preceptor's Primer for Bayesian Data Science: Using the Cardinal Virtues for Inference* and the matching learnr tutorials in the `primer.tutorials` package. It is addressed to Claude. David Kane is the author; Claude is the co-author he collaborates with to produce new material.

The goal is that this file is the only reference either of us needs when starting work on a new chapter/tutorial pair. When something in another document (the `cardinal_virtues.qmd` vignette, the `tables.qmd` vignette, the `04-cardinal-virtues.qmd` chapter, or `template_tutorial.Rmd`) conflicts with what is written here, this file wins.

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

---

## 2. Working with David

The authoring of a chapter/tutorial pair is a conversation. Do not try to produce a finished chapter in one shot. The rough protocol:

1. **David picks the topic or gives a pointer.** Example: "Write chapter 9, on logistic regression."
2. **Claude proposes the framing.** Candidate dataset(s), the Imagine-that-you-are scenario, the broad question, a specific narrow question (the QoI), and whether the tutorial will use a predictive or causal model. Offer two or three options where there is real choice.
3. **David picks.** Iterate on the dataset, the unit, the outcome, the treatment (if causal), and a short list of covariates.
4. **Claude drafts the Preceptor Table and Population Table** as gt code. David reviews and corrects.
5. **Claude drafts the chapter Wisdom section**, then the tutorial Wisdom section. David reviews. Repeat by virtue: Justice, Courage, Temperance.
6. **Claude checks spaced-repetition coverage** against the tutorial index in §13 and adjusts which recurring questions this tutorial asks.

This protocol is a default; deviate when it makes sense. Where a decision is small and reversible (phrasing of a knowledge drop, which concrete example to use in an exercise), just make it. Where a decision shapes the rest of the chapter (dataset, QoI, functional form), pause and ask.

When you pause to ask, make it easy for David to answer: short list of options, your recommendation, your reasoning. Do not ask open-ended questions when a multiple-choice question will do.

---

## 3. Output artifacts and file conventions

Chapters are Quarto files with the top-level `#` already set by the book structure; use `##` for virtue-level sections. Filename convention: `NN-topic-name.qmd` where `NN` is the chapter number (e.g. `04-cardinal-virtues.qmd`, `09-logistic-regression.qmd`).

Tutorials are R Markdown files with `learnr::tutorial` output. They live in the `primer.tutorials` package under `inst/tutorials/NN-topic-name/tutorial.Rmd` (or similar — confirm with David before writing the first one in a new package state). The tutorial's `id` in the YAML is `NN-topic-name`, lowercase, dashes for spaces. See `template_tutorial.Rmd` for the full YAML header.

For new chapters, produce a single `.qmd` file. For new tutorials, produce a single `.Rmd` file that follows the structure of `template_tutorial.Rmd`. Do not emit partial diffs; produce complete files David can drop in place.

---

## 4. Chapter structure

Every example chapter has six top-level sections under `#`:

1. **Introduction** — `##`-level. Names the four Cardinal Virtues. Gives one "Imagine that you are…" paragraph motivating the problem. Names the dataset. Typically 2–6 paragraphs.
2. **Wisdom** — question, Preceptor Table (predictive), EDA, Preceptor Table (causal), the validity decision.
3. **Justice** — Population Table, stability, representativeness, unconfoundedness (for the causal model).
4. **Courage** — mathematical structure, candidate models, tests, the selected Data Generating Mechanism. In later chapters, a posterior predictive check.
5. **Temperance** — interpretation, questions and answers, humility.
6. **Summary** — one final graphic, one concluding paragraph, and the sentence "The world is always more uncertain than our models would have us believe."

Chapters include full image references (`knitr::include_graphics("other/images/Wisdom.jpg")` etc.) at the top of each virtue section. Chapters quote extensively — from Tukey, from Rumsfeld, from the Bible, from whomever fits. Quotes are good; use them.

Chapters use the predictive model first and the causal model second where both apply. Both models share the dataset but not the question.

---

## 5. Tutorial structure

Every example tutorial has the same six sections as a chapter (Introduction, Wisdom, Justice, Courage, Temperance, Summary), but each section is a sequence of `### Exercise N` blocks rather than prose.

Tutorials are self-contained. A student who has not read the chapter should be able to complete the tutorial and learn everything the tutorial is trying to teach. This means every tutorial defines the key terms (causal effect, Preceptor Table, Population Table, validity, stability, representativeness, unconfoundedness, DGM), even though the chapter does too.

Use `template_tutorial.Rmd` as the structural skeleton. The YAML header, the setup chunk, the child documents (`copy_button.Rmd`, `info_section.Rmd`, `download_answers.Rmd`), and the per-section exercise patterns in the template are all load-bearing. Do not reinvent them.

Every tutorial fits **one model**, not two — either predictive or causal, matching whichever the chapter spends more time on.

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

Student writes R code in the exercise chunk. Has a hint chunk and a test chunk. The test chunk contains the canonical answer; the exercise chunk is empty (or has scaffolding). Code exercises usually build a pipeline line-by-line across several exercises, so that each intermediate step produces visible output the student can examine.

### 7.2 Written exercise with model answer

A `question_text()` with `message = "..."` containing the canonical answer, `allow_retry = FALSE`, `incorrect = NULL`. Used for questions that have a correct answer — definitions, conceptual framings, recall. Students see our answer after submitting theirs.

The text in `message` is read closely by students, who compare their answer to ours. Our answer must be excellent. Use the wording in §11 verbatim for definitional questions.

### 7.3 Written exercise without model answer

A `question_text()` with no `message` (or equivalently, an empty answer), `allow_retry = TRUE`, `try_again_button = "Edit Answer"`. Used only when there is no single correct answer — typically when the student is asked to run a diagnostic command like `show_file()` and paste the output, or to describe something specific to their own analysis. Do not use this type for definitional or conceptual questions.

---

## 8. Spaced repetition

We believe spaced repetition works, and we care more about repetition than precise spacing. The goal: three months after finishing the tutorials, a student can still answer questions about our key definitions.

**Example chapters** repeat all definitions and concepts every time — each chapter is self-contained.

**Example tutorials** do not repeat everything every time; doing so would make each tutorial too long. Instead, use a schedule like: ask in tutorials 1, 2, 3, skip tutorial 4, ask in 5, skip 6 and 7, ask in 8, skip 9 and 10, ask in 11, and so on. Exact cadence can vary; the principle is "ask often early, then space out."

Before writing a tutorial, read §13 to see which recurring questions are due this tutorial.

---

## 9. AI-mediated code exercises

Students use AI (ChatGPT, Claude, etc.) to produce code. They prompt the AI and paste the result into their tutorial. Design code exercises accordingly:

- **Do not build pipelines line-by-line with the student writing each line** the way older tutorials do. Instead, state the goal of the code clearly enough that a student could prompt an AI for it, then have them run the AI's output.
- **Be explicit about what the code should do, not what it should look like.** "Create a tibble called `x` that contains only the 1992 observations from `nes`, with no missing values." is better than a fill-in-the-blanks pipeline.
- **Knowledge drops still matter.** The student may not read the code the AI wrote carefully. Use the End of each exercise to point out what the code actually did and why it matters.

This affects the shape of Wisdom and Courage sections in particular. Wisdom has lots of "examine the data" exercises; those still make sense as AI-prompted code. Courage fits models; fitting is typically one-shot, so most of Courage is interpretation, not pipeline-building.

---

## 10. Preceptor Table and Population Table format

Every Preceptor Table and Population Table in the book and the tutorials follows the same format. You can generate them directly as `gt` code; you do not need to call `primer.tutorials::make_p_tables()` (that function exists for humans authoring by hand).

### 10.1 Preceptor Table

Four rows. Columns grouped under spanners:

- **Unit/Time** — two columns: a unit label (e.g., `Senator`, `Candidate`, `Student`) and a time label (e.g., `Session Year`, `Election Year`).
- **Potential Outcomes** (causal) or **Outcome** (predictive) — two or more columns for causal, one column for predictive. Causal column names name the counterfactual, e.g. `Lifespan if Win`, `Lifespan if Lose`.
- **Treatment** (causal only) — one column.
- **Covariates** — one or more columns plus a final `More` column that is always `"..."`.

Row 3 is blank (all `"..."`). Rows 1, 2, and 4 are concrete examples. All cell values are in double quotes, including numbers (e.g. `"42"`). Where a potential outcome is not observed, use `"?"`.

### 10.2 Population Table

Eleven rows. Same column structure as the Preceptor Table, plus a leading `Source` column with values `"Data"`, `"Preceptor"`, or `"..."`.

Row layout:

1. blank (all `"..."`, including `Source`)
2–5. four data rows (row 4, the middle, is blank; `Source` is still `"Data"` where content applies and `"..."` on the blank row)
6. blank
7–10. four Preceptor rows (row 9, the middle, is blank; same Source convention)
11. blank

Five footnotes per table, attached via `gt::tab_footnote()`:

- Title footnote — states the question.
- Unit footnote — defines what each row represents. Connects to stability and representativeness.
- Outcome footnote — for causal, connects to validity and explains what the potential outcomes mean; for predictive, describes the outcome variable and its measurement.
- Treatment footnote — defines the treatment and connects to unconfoundedness. (Omit for predictive.)
- Covariates footnote — explains the covariate set and what the `More` column represents.

### 10.3 gt template

Use `gt::tab_spanner()` for the column groups, `gt::cols_align("center", everything())` with `cols_align("left", ...)` for the unit column, and explicit `cols_width()` in `px` for legibility. Render `More` as `...` via `gt::cols_label(More = "...")`. Apply `gt::fmt_markdown(everything())` so cell contents can include emphasis.

A worked causal example for reference is in §5 of the `tables.qmd` vignette (gubernatorial elections and longevity). When in doubt, match that layout.

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

## 12. Canonical knowledge drops

Short prose fragments that recur between exercises across many tutorials. Use verbatim where applicable.

**On validity as a columns-thing.**
> *Validity is always about the columns in the Preceptor Table and the data. Just because columns from these two different tables have the same name does not mean that they are the same thing.*

**On stability as a time-thing.**
> *Stability is all about time. Is the relationship among the columns in the Population Table stable over time? The longer the time period between the data and the Preceptor Table, the more suspect stability becomes.*

**On representativeness' cost.**
> *When representativeness is violated, the estimates for the model parameters will be biased.*

**On the crispest summary.**
> *Validity is about the columns in our Population Table. Stability and representativeness are about the rows.*

**On the unconfoundedness shortcut.**
> *The easiest way to ensure unconfoundedness is to assign treatment randomly.*

**On stability vs. representativeness.**
> *Stability looks across time periods. Representativeness looks within time periods.*

**On no hypothesis tests.**
> *Null hypothesis testing is a mistake. There is only the data, the models, and the summaries therefrom.*

**On the world's uncertainty.**
> *The world is always more uncertain than our models would have us believe.* (Last line of every chapter.)

**On humility.**
> *We can never know the truth.*

---

## 13. Curriculum and tutorial index

This section is a registry. It tracks what chapters exist, what datasets they use, and which recurring questions each tutorial has asked. It is how we execute spaced repetition in §8.

**Status: not yet populated.** Before writing a new tutorial, David and Claude should fill in the rows for existing tutorials so that Claude knows what has and has not been asked recently.

| Chapter | Type | Topic | Dataset | Model (tutorial) | Recurring questions asked |
|--------:|------|-------|---------|------------------|---------------------------|
| 01 | misc | Mechanics | — | — | — |
| 02 | misc | Probability | — | — | — |
| 03 | misc | Sampling | — | — | — |
| 04 | misc | Cardinal Virtues | — | — | — |
| 05 | misc | Rubin Causal Model | — | — | — |
| 06+ | example | TBD | TBD | TBD | TBD |

(David: please fill this in, or tell Claude which existing file to read it off of.)

---

## 14. R tooling

The tutorial setup chunk typically loads, in this order:

```
library(learnr)
library(tutorial.helpers)
library(gt)
# Students would load the below themselves in the Console:
library(tidyverse)
library(tidymodels)        # or ordinal, or another modeling package
library(broom)             # or broom.mixed
library(marginaleffects)
library(easystats)         # used for check_predictions()
```

Setup chunks should never contain code that takes more than a few seconds to run. See the `tutorial.helpers` AI article (`https://ppbds.github.io/tutorial.helpers/articles/ai.html`) for background. If a model needs to be fit in setup, use a cheap placeholder fit; anything expensive should be cached or skipped.

Every tutorial's setup chunk fits the tutorial's model and assigns it to a name starting with `fit_` (e.g. `fit_attitude`). Use the same name throughout the tutorial.

For chapters, the setup is simpler: load the packages, fit the model, move on.

---

## 15. What still lives in `template_tutorial.Rmd`

The template remains ground truth for:

- The full YAML header, including `learnr::tutorial` output options and `runtime: shiny_prerendered`.
- The three child-document references (`copy_button.Rmd`, `info_section.Rmd`, `download_answers.Rmd`) and where they go.
- Exact phrasing of operational instructions students follow (creating a GitHub repo, `Cmd/Ctrl + Shift + K` to render, `show_file()` usage, "CP/CR" conventions).
- Recurring operational exercises in the Introduction section that teach the QMD-and-Console workflow.
- Any definitions or canonical knowledge drops not yet migrated into §11 or §12.

When writing a new tutorial, start from `template_tutorial.Rmd`, replace every `XX`, and use §11 and §12 of this file as the ground truth wherever those sections cover what the template covers.

When you find a definition or knowledge drop in the template that ought to be canonical but is not in §11 or §12, propose adding it here.

---

## 16. Open items

Things flagged but not yet resolved. Revisit when relevant.

- **Assignment mechanism and selection mechanism** are not yet defined here or in the vignette as first-class concepts distinct from unconfoundedness. The template's bottom TODO list asks for Justice exercises on them. When ready, add definitions to §11 and decide where they fit in the Justice flow.
- **Mathematics** appears in the `cardinal_virtues.qmd` Justice skeleton but is not written anywhere. Decide whether it is its own Justice subsection or folded into Courage.
- **`04-cardinal-virtues.qmd` is inconsistent with this file** on where validity lives. The chapter places validity in Wisdom (as a walk-away decision); this file places it in Justice (as one of four assumptions). Until the chapter is updated, prefer the framing in §11 of this file.
- **Functional form guidance** for Courage — the rule of thumb is continuous outcome → linear, binary → logistic, multi-category → multinomial logistic. Add a richer §7/§14-style guide when writing chapters that venture beyond this.
- **Posterior predictive checks** via `easystats::check_predictions()` — appears in setup but is not yet documented as part of Courage's "check your model" step. Codify when writing the first chapter that uses it.
- **AI tool article** at `https://ppbds.github.io/tutorial.helpers/articles/ai.html` — worth absorbing the relevant rules into §9 the next time we touch it.