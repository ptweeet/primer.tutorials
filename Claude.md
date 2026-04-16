# Overview

This file serves as guidance for the Preceptor's Primer project. The project includes a textbook *Preceptor's Primer for Bayesian Data Science: Using the Cardinal Virtues for Inference*, this collection of R learnr tutorials, with one tutorial for each chapter, and some yet to be created material for classroom use.

Our purpose is to teach students how to be data scientists. A data scientists is someone who, when confronted by a question and some data which might be used to answer that question, can complete a series of steps which, we hope results in an answer to that question, presented graphically, and including a measure of uncertainty.

We try to help students remember the steps by organizing them around the theme of the [Cardinal Virtues](https://en.wikipedia.org/wiki/Cardinal_virtues). Indeed, the more references and allusions we can make to the Cardinal Virtues, the better.

For more background, please read template_tutorial.Rmd in this directory and https://ppbds.github.io/tutorial.helpers/articles/ai.html.


# Chapters and Tutorials

Every student is forced to complete the tutorials. We want students to read the chapters, but fewer than one quarter will probably do so. That means that the tutorials can not assume that a student has read the chapter. Tutorials must be self-contained. Chapters cover all the same material as in its associated tutorial, but also add much more. They do more data exploration. They create more models. They experiment with more approaches to exploring the question. Almost every piece of code which is executed in the tutorial is also executed in the chapter, but that is just the start.

Chapters, and the tutorials associated with them, come in two types. 

First, we have *example* chapters/tutorials which follow the Cardinal Virtues, working through a well-defined data science problem. Earlier chapters/tutorials are much simpler than later ones. Indeed, a key goal is to have the chapters/tutorials slowly increase in sophistication as the students become more practiced in doing data science. The example chapters/tutorials are all very similar to each other in the structure.

Second, we have *miscellaneous* chapters/tutorials which cover a variety of topics but which do not include a major data science exercise. There are currently five of these chapters/tutorials: Probability, Sampling, Rubin Causal Model, Cardinal Virtues, and Mechanics. 

Example chapters always cover every important topic. In many ways, they are self-contained. They always define all the key terms: causal effect, Preceptor Table, Population Table and so on. They (almost) always discuss important concepts like hypothesis testing, posterior predictive checks, et cetera. (Earlier chapters might not have an explicit discussion of advanced concepts. We sometimes wait to introduce such concepts to later, but, after the first introduction, they appear in every subsequent chapter.

Example chapters are much longer than tutorials. For example, every example chapter includes, with reference to the same data set, a predictive model and a causal model. This means that each chapter will also have two Precepor Table and two Population Tables. A tutorial only has the time for one model. That model is also covered, in more detail, in the associated chapter. The other sort of model in the chapter is all extra material, not covered in the tutorial.

## Spaced repetition

We believe that spaced repetition works. We also believe that some definitions/concepts are more important than others. Our goal is that, after completing, three months after completing the tutorials, a student can still answer questions about the definitions/concepts we cover. 

In the exercise chapters, we always repeat all the definitions and concepts. Each exercise chapter is self-contained in that way.

In the exercise tutorials, we do not ask about every definition/concept each and every time. Doing so would make the tutorials too long! Instead, we use spaced repetition. We might ask for the definition of a causal effect in the first three exercise tutorials, then skip a tutorial, then ask it again, then skip two tutorials, then ask it again, and then skip two tutorials, and so on.


## template_tutorial.Rmd

The most important document for you to read is the template_tutorial.Rmd. It was the template which I used to create exercise tutorials in the past. It is good, for four main reasons.

First, it contains many excellent questions which should be used in multiple exercise tutorials. The only way to ensure that students remember the definition of a causal effect is to ask them the definition many times, using spaced repetition.

Second, it contains many excellent answers to those questions. We should always provide the exact same answer to questions about definitions. The template tutorial is the ground truth of those definitions, at least until we move them into this document.

Third, it contains good knowledge drops. That is, 


## Question flow

Each exercise should have a flow which requires that students hit the “Continue” button at least once.

Begin with a Start which is a sentence or two of knowledge and/or the question itself. If the length of the Start text is longer than one or two lines, then do not place the question code chunk in the same part. Instead, the Start includes a triple hash, thereby creating the Continue button. If the length of the text is short enough that students are willing to read it (at most two sentences), you can include the question code chunk in the same part.

Most of the time there is no need for a triple hash before the exercise code chunk.

Do not expect students to read more than two sentences of text at a time. After two sentences, you almost always want to use a triple hash in order to create a Continue button so that students have a break. They won’t read more than two sentences without a break.

There is a danger that students will just click the Continue button until they see a question and, only then, start reading. There is little we can do about that. However, we can take advantage of students’ tendency to read the sentence or two which proceeds the question fairly closely. This is a great place for teaching since students can’t skip it since they don’t know if it provides necessary context for answering the question.

After the Start, come the exercise code chunks. Recall that, as the tutorial.helpers package explains, 

The exercise code chunk is the location in which students will place their answers.
The hint code chunk includes any hints for the students. Hint code chunks are only available for code exercises. The code chunk name in the hint code chunk is always exactly the same as the one for the exercise code chunk, except with -hint-n attached at the end. The n is replaced by the number of the hint. Almost always, there is only one hint, so the suffix is -hint-1. We always set eval = FALSE in the hint code chunk since, often, the hint will not be legal R code.
The test code chunk has exactly the label as the one for the exercise code chunk, except with -test attached at the end. Test code chunks, like hint code chunks, are only used for code exercises. It always includes the include = FALSE code chunk option because we never want to show the code or the results to students. Instead, the purpose of the test code chunk is to ensure that the correct answer — that is, the code we want students to enter into the exercise code chunk — works.
The three code chunks are always followed by a triple hash. We want a student to pause after she has submitted her answer so that she is more likely to consider the output from her submission before moving on.

The last part of an exercise is the end, our main opportunity to drop some knowledge.

The last part of the topic is another knowledge drop. It is not another exercise. It is just a knowledge drop after the last exercise which tries to take a broader overview. It is often separated from that last exercise by a simple ###. Again, this can’t be more than a sentence or two. But it should be more substantive than a simple “Good job.” For example, if the topic has involved creating a scatter plot, then the last exercise will be putting the final touches on that scatter plot. The last knowledge drop should be something about scatter plots in general, not a minor point about the particular scatter plot which the student has just created.

# Definitions

This is the ground truth for our key definitions and concepts.

[Rubin Causal Model](https://en.wikipedia.org/wiki/Rubin_causal_model) is an approach to the statistical analysis of cause and effect based on the framework of potential outcomes.

## Preceptor Table

> A Preceptor Table is the smallest possible table with rows and columns such that, if there is no missing data, our question is easy to answer.

**Predictive Models and Causal Models** are different because predictive models have only one outcome column. Causal models have more than one (potential) outcome column because we need more than one potential outcome in order to estimate a *causal effect*. The first step in a data science problem is to determine if your QoI requires a causal or a predictive model.

If you don't care what Joe would have done in a counter-factual world in which he got a different treatment, if all you care about is predicting what Joe does *given the treatment he received*, then you just need a predictive model.

**Units** are determined by the original question, which also determines the quantity of interest. They are the **rows**, both in the *Preceptor Table* and in the data. 

**Variables** is the general term for the **columns** in both the Preceptor Table and the data. In fact, the term is even more general since it may refer to data vectors which we would like to have in order to answer the question but which are, sadly, not available in the data. The columns in the data are a subset of all the variables in which we might be interested.

**The outcome** is the most important variable. It is determined by the question/QoI. By definition, it must be present in both the data and the Preceptor Table. Different problems might be answered with the same data set, with different variables playing the role of the outcome in each case.

**Covariates** is the general term for all the variables which are not the **outcome**. As with **variables**, there are three different contexts in which we might use the term covariates. First, covariates are all the variables which might have some connection with our outcome, even if they are not included in the data. Second, covariates are all the variables in the data other than the outcome.  Third, covariates can refer to just the subset of the variables in the data which we actually use in our model. The second usage is, obviously, a subset of the first, and the third usage is a subset of the second.

**Units**, **outcomes** and **covariates** are important parts of every data science model. Causal, but not predictive, models also include at least one **treatment**, which is just a covariate which we can, at least in theory, manipulate. The QoI determines the units and outcomes for your model.

**Potential Outcome** is the outcome for an individual under a specified treatment. A *potential outcome* is just a regular outcome in the case of a causal model. In a predictive model, we just have an outcome. It is just another variable, the one that, in the context of this problem, we are interested in explaining/modeling/predicting. In a causal model, on the other hand, there are at least two outcomes: the outcome which happens if the unit gets the treatment and the outcome which happens if that same unit gets the control. We refer to both of these outcomes as *potential outcomes*.


**Causal Effect** is the difference between two potential outcomes.




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
