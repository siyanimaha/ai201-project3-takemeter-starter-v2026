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
| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.200 | 0.567 | +0.367 |
| Macro F1 | 0.178 | 0.358 | +0.180 |
| F1 — `analysis` | 0.133 | 0.700 | +0.567 |
| F1 — `hot_take` | 0.143 | 0.375 | +0.232 |
| F1 — `reaction` | 0.258 | 0.000 | -0.258 |

**What I predicted before I looked:**

I predicted that my trained model would have higher overall accuracy and F1 scores than the baseline because it was trained specifically on my labeled movie posts.

**What the gap actually means:**

My trained model performed better overall than the baseline. The biggest improvement was for `analysis`, where the F1 score increased from 0.133 to 0.700. The model also improved on `hot_take`. However, `reaction` dropped from 0.258 to 0.000, which shows that my trained model still has trouble identifying reaction posts.

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.567 | 0.633 | 0.667 | 0.100 |
| Macro F1 | 0.358 | 0.441 | 0.476 | 0.118 |
| F1 — `analysis` | 0.700 | 0.722 | 0.714 | 0.022 |
| F1 — `hot_take` | 0.375 | 0.600 | 0.714 | 0.339 |
| F1 — `reaction` | 0.000 | 0.000 | 0.000 | 0.000 |

Accuracy moved 0.100 across seeds. Enough that a criterion landing within that band can't really be called either way.

### Confusion matrix

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 14 | 0 | 0 |
| **hot_take** | 9 | 3 | 0 |
| **reaction** | 3 | 1 | 0 |

**My biggest off-diagonal number, and what it means:**

The biggest confusion was that 9 real `hot_take` posts were predicted as `analysis`, while 0 real `analysis` posts were predicted as `hot_take`. This shows that the model leans toward predicting `analysis` when it has trouble telling the difference between `hot_take` and `analysis`.

---

## Verdicts and Diagnoses

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | At least 0.70 across all three seeds | MISSED | Accuracy was 0.567, 0.633, and 0.667. All three seeds were below 0.70. |
| 2 | Per-label F1 | At least 0.60 for every label across all three seeds | MISSED | `reaction` had an F1 of 0.000 in all three seeds. `hot_take` also had an F1 of only 0.375 for Seed 42. |
| 3 | Label balance | No single label should make up more than 60% of the 200 posts | MET | The largest label was `analysis` with 93 of 200 posts, or 46.5%, which is below 60%. |
| 4 | Label agreement | At least 75% agreement with another labeler | MET | I agreed with the staff labels on 30 out of 30 posts, giving me 100% agreement. |
| 5 | High-confidence accuracy | Predictions with confidence of at least 0.80 should be correct at least 80% of the time across all three seeds | NOT DIRECTLY MEASURABLE | The evaluation reports accuracy for the most-confident third, not predictions with confidence of at least 0.80. The most-confident-third accuracy was 0.80, 0.90, and 1.00 across the three seeds, but this is not the exact measurement in my original criterion. |

**Diagnoses**

**Criterion 1 — Overall accuracy:**  
The model missed the 0.70 accuracy target across all three seeds. A major problem is the `reaction` label. The model did not correctly identify reaction posts, which lowered the overall performance. My 100% agreement with the staff labels suggests that my labeling rules are consistent, so this looks more like a model/data problem than a labeling consistency problem.

**Criterion 2 — Per-label F1:**  
The biggest problem is `reaction`, which had an F1 of 0.000 for all three seeds. `reaction` is also my smallest label with only 24 examples, compared with 93 `analysis` and 83 `hot_take` examples. This gives the model much less reaction data to learn from. My 100% staff agreement also suggests that the main problem is not that I am applying the label definitions inconsistently.

**Criterion 5 — High-confidence accuracy:**  
I cannot give the original criterion a fair MET or MISSED verdict because my evaluation does not calculate accuracy specifically for predictions with confidence of at least 0.80. It calculates accuracy for the most-confident third instead. Those results were 0.80, 0.90, and 1.00, which show that more-confident predictions were generally more accurate, but they do not directly test my original threshold.

---

## Agreement Report
## Agreement Report

**Agreement rate:** **30 / 30 = 100%**

**Disagreements**

There were no disagreements. I matched the staff label on all 30 posts.

**What the pattern in my disagreements tells me:**

Since I had no disagreements with the staff labels, the staff taxonomy and decision rules were clear for this set. This also suggests that inconsistent labeling was not the main cause of my model's poor performance on `reaction`.


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
