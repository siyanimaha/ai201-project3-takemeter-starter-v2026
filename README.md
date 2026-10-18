# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

This project uses posts from the public r/movies community on Reddit. My classifier sorts movie discussion into three categories: `analysis`, `hot_take`, and `reaction`. The goal is to distinguish between opinions that give supporting reasons, strong opinions with little explanation, and posts that mainly express an immediate emotional response.

---

## Label Taxonomy

### `analysis`

**Definition:** A movie opinion that explains why using specific reasons, examples, or details.

**Example 1:** "Electric Dreams would probably have to be up there. Very forgettable 80s romance but one of the best songs."

**Example 2:** "The Gary Jules version of Mad World for the movie Donnie Darko. Great movie but I don't think it's anywhere as well known as that song."

### `hot_take`

**Definition:** A strong or confident movie opinion with little or no explanation or support.

**Example 1:** "That film is an underrated classic, if only to see Gene Kelly on roller skates again."

**Example 2:** "St. Elmo's Fire. Great song, bad movie nobody talks about anymore."

### `reaction`

**Definition:** A response that mainly expresses a feeling or immediate personal response to a movie.

**Example 1:** "I've never seen the movie and I listen to the soundtrack all the time."

**Example 2:** "I didn't even know it was from a movie!"

### The hardest boundary

**Which two labels:** `hot_take` and `reaction`

**The decision rule I used every time:** If the post makes a strong judgment about the quality or value of a movie without supporting reasons, I label it `hot_take`. If the post mainly describes the person's emotional response or feeling, I label it `reaction`. If the post supports its judgment with a specific reason, example, or detail, I label it `analysis`.

---

## The Dataset

I collected 200 public comments from the r/movies community on Reddit. I read movie-related discussions and organized the comments into my three labels. AI was used to help pre-label the dataset, and I reviewed the labels using my definitions and decision rules.

### Label counts

- `analysis`: 93
- `hot_take`: 83
- `reaction`: 24

The dataset is somewhat imbalanced because reaction posts were less common than analysis and hot takes in the discussion I collected from.

### Three hard cases

1. **Post:** "That film is an underrated classic, if only to see Gene Kelly on roller skates again."
   - **Possible labels:** `hot_take` or `analysis`
   - **My label:** `hot_take`
   - **Why:** It gives a strong judgment that the movie is underrated, but it does not give enough detailed support to count as analysis.

2. **Post:** "I can't hear that song without pumping my fist in the air, so I don't think it qualifies."
   - **Possible labels:** `reaction` or `hot_take`
   - **My label:** `hot_take`
   - **Why:** It includes a personal reaction, but the main point is the judgment that the song does not qualify.

3. **Post:** "Scared Tragically hip. It was in the trailer park boys movie. I like the movie but that song is amazing."
   - **Possible labels:** `reaction` or `hot_take`
   - **My label:** `reaction`
   - **Why:** The post contains an opinion, but it mainly expresses the person's personal response to the movie and song.

---

## The Training Run

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-5`, batch size 16, maximum length 128, seed 42.

**Anything I changed from the defaults, and why:** I kept the default training settings because I wanted to establish a baseline training run before making any changes.

**Split sizes:** 139 training posts, 31 validation posts, and 30 test posts.

The test split contained:
- `analysis`: 14
- `hot_take`: 12
- `reaction`: 4

The `reaction` label had only 4 examples in the test split, so its performance is less stable and should be interpreted carefully.

### Results

- Overall accuracy: 0.567
- Macro F1: 0.358
- `analysis` F1: 0.700
- `hot_take` F1: 0.375
- `reaction` F1: 0.000
- Most-confident third accuracy: 0.800
- Least-confident third accuracy: 0.400

---

## How I Used AI

**Moment 1**

- **What I asked for:** I used AI when I needed clarification about the differences between some of my labels.
- **What came back:** AI explained the differences between `analysis`, `hot_take`, and `reaction` and gave suggestions for handling borderline cases.
- **What I changed:** I used the suggestions as guidance while following my own label definitions and decision rules.

**Moment 2**

- **What I asked for:** I used AI to help me understand parts of the project instructions and notebook setup.
- **What came back:** AI explained unfamiliar steps and helped me understand the notebook output.
- **What I changed:** I followed the assignment requirements and used the explanations to complete the setup and training process.

**Pre-labelling disclosure:** I used AI to assist with pre-labelling the collected posts. I personally reviewed the classifications using my label definitions and decision rules.
<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
